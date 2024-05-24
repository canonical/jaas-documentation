JAAS: Deploy on K8S
===================

Introduction 
------------

In this how-to we will be deploying JIMM v3 and all supporting components on Kubernetes, specifically MicroK8s. JIMM - Juju Intelligent Model Manager provides the ability to manage multiple Juju models from a single place.

This tutorial will teach you about JIMM and Juju as well as a bit about Canonical's identity bundle and other supporting dependencies.

Prerequisites
-------------

For this tutorial you will need the following:

- An Ubuntu machine.

Setup Multipass (Optional)
--------------------------
Multipass is a tool to launch Ubuntu VMs from Windows, Linux and MacOS. The remainder of this guide can be run from within a Multipass VM to avoid affecting the host machine.

Start by running the following commands to install and start a Multipass VM, the optional section will define the VM's memory/CPU/disk usage.

.. code:: bash

    sudo snap install multipass
    multipass launch jammy --name jimm-deploy [-m 12g -c 4 -d 40G]
    multipass shell jimm-deploy

Setup Juju & MicroK8s
---------------------
Now we can install our dependencies, note that Juju 3+ only works with a strictly confined MicroK8s Snap.

.. note::
    JIMM supports talking to many different Juju controller versions. However changes to the Juju CLI to support authentication with JIMM 
    have only been added in Juju 3.5. For this reason, ensure you are running the Juju CLI with version 3.5 for the best experience.

.. note::
    JIMM can be deployed by any Juju controller that supports Juju secrets, i.e. Juju 3+

.. code:: bash

    sudo snap install microk8s --channel=1.28-strict/stable
    sudo snap install juju --channel=3.5/stable

Once you have the Juju CLI installed, you will need to bootstrap a Juju controller to your cloud. 
We will be using MicroK8s as our cloud. The Juju documentation has detailed instructions on how to bootstrap a controller
for various clouds and machine types.

To begin, run the following commands to setup MicroK8s.

.. code:: bash

    # Add the 'ubuntu' user to the MicroK8s group:
    sudo usermod -a -G snap_microk8s ubuntu
    # Give the 'ubuntu' user permissions to read the ~/.kube directory:
    sudo chown -f -R ubuntu ~/.kube
    # Create the 'microk8s' group:
    newgrp snap_microk8s
    # Enable the necessary MicroK8s addons:
    sudo microk8s enable hostpath-storage dns ingress
    # Setup the metallb add-on for the identity bundle later
    sudo microk8s enable metallb:10.64.140.43-10.64.140.100
    # Set up a short alias for the Kubernetes CLI:
    sudo snap alias microk8s.kubectl kubectl

Next, bootstrap your Juju controller.

.. code:: bash

    # Since the Juju package is strictly confined, you also need to manually create a path:
    mkdir -p ~/.local/share
    juju bootstrap microk8s jimm-demo-controller

Deploy the identity-bundle
--------------------------
For this tutorial we will use Canonical's identity bundle to provide authentication. JIMM uses OAuth 2.0, a provider agnostic way of handling authentication.
Although any compliant identity provider could be used with JIMM, we recommend the use Canonical's identity platform for the best compatibility.
Canonical's identity bundle uses Ory Hydra/Kratos to provide an OAuth server and user management, respectively.

Now we will create a Juju model for the identity platform and deploy the bundle.

.. code:: bash

    juju add-model iam
    juju deploy identity-platform --trust --channel 0.2/edge

Watch the deployment by running:

.. code:: bash

    juju status --watch 1s

Eventually all application should reach an ``active`` state except for the ``kratos-external-idp-integrator`` application. This application allows you to connect your identity platform
to an external identity provider like Google, GitHub, Microsoft, etc. This is necessary because the identity provider only acts as an identity broker.

We recommend using the following `how-to <https://charmhub.io/topics/canonical-identity-platform/how-to/integrate-external-identity-provider>`__ and choosing your preferred identity provider.
Note that you can temporarily skip this step and return to it later, JIMM can still run without integrating the identity bundle to an external identity provider but login to JIMM will not work.

Now run the following commands to create offers that will be consumed in the next step.

.. code:: bash

    juju offer hydra:oauth
    juju offer self-signed-certificates:send-ca-cert

Running ``juju status`` should now two offers that we will use from a different model in the next step.

Deploy JIMM
-----------
Now we will deploy JIMM and its dependencies into a new model. Let's first explore however what JIMM's dependencies are and what they are used for.

- OpenFGA: The OpenFGA charm provides authorisation, defining who is allowed to access what.
- PostgreSQL: PostgreSQL is JIMM's database of choice and stores persistent state. This PostgreSQL instance is used by both JIMM and OpenFGA.
- Vault: The Vault charm is used for storing sensitive user secrets. JIMM can be configured to store in plain-text in PostgreSQL but this is not recommended for a production environment.
- Ingress: There are various charms that provide ingress into a K8s cluster. JIMM supports `Traefik Ingress <https://charmhub.io/traefik-k8s>`__ and `Nginx Ingress Integrator <https://charmhub.io/nginx-ingress-integrator>`__, this tutorial will use the latter.

.. note::
    In a production environment you may want to structure your deployment slightly differently.  
    You might consider placing your database on a VM and performing a cross-model relation.  
    You might also consider deploying a central Vault and relating to it cross-model.

Let's begin by creating a new model for JIMM and deploying the necessary applications:

.. code:: bash

    juju add-model jimm
    # The channel used for the JIMM charm is currently 3/edge.
    # At a later date this will be promoted to the 3/stable channel.
    juju deploy juju-jimm-k8s --channel=3/edge jimm
    juju deploy openfga-k8s --channel=2.0/stable openfga
    juju deploy postgresql-k8s --channel=14/stable postgresql
    juju deploy vault-k8s --channel=1.15/beta vault
    juju deploy nginx-ingress-integrator --channel=latest/stable --trust ingress
    juju relate jimm:nginx-route ingress
    juju relate jimm:openfga openfga
    juju relate jimm:database postgresql
    juju relate jimm:vault vault
    juju relate openfga:database postgresql
    
At this point only OpenFGA and PostgreSQL should be in an active state.
JIMM, Vault and the ingress should all be in a blocked state. Next we will relate JIMM to the cross-model offers we created previously.

.. code:: bash

    juju relate jimm admin/iam.hydra
    juju relate jimm admin/iam.self-signed-certificates

Before we move on we will deploy our own self-signed-certificates operator in order to eventually use JIMM with HTTPS.
We are doing this step afterwards to avoid issues that occur when performing the relations before the ingress is ready.

.. code:: bash
    
    juju deploy self-signed-certificates jimm-cert
    juju relate ingress jimm-cert

Now move onto the next step to initialise Vault.

Initialise Vault
----------------
The Vault charm has documentation on how to initialise it `here <https://charmhub.io/vault-k8s/docs/h-getting-started?channel=1.15/beta>`__. But an abridged version of the steps are provided here.

Install the Vault CLI client and the ``yq`` tool.

.. code:: bash

    sudo snap install vault
    sudo snap install yq

To communicate with the Vault server we now need to setup 3 environment variables:

- ``VAULT_ADDR``
- ``VAULT_TOKEN``
- ``VAULT_CAPATH``

Run the following commands to setup the first two variables that will enable communication with Vault.

.. code:: bash

    export VAULT_ADDR=https://$(juju status vault/leader --format=yaml | yq '.applications.vault.address'):8200; echo "Vault address =" "$VAULT_ADDR"
    cert_juju_secret_id=$(juju secrets --format=yaml | yq 'to_entries | .[] | select(.value.label == "self-signed-vault-ca-certificate") | .key'); echo "Vault ca-cert secret ID =" "$cert_juju_secret_id"
    juju show-secret ${cert_juju_secret_id} --reveal --format=yaml | yq '.[].content.certificate' > vault.pem && echo "saved certificate contents to vault.pem"
    export VAULT_CAPATH=$(pwd)/vault.pem; echo "Setting VAULT_CAPATH from" "$VAULT_CAPATH"

Verify that Vault is accessible.

.. code:: bash

    vault status

The output should resemble the following

.. code::

    Key                Value
    ---                -----
    Seal Type          shamir
    Initialized        false
    Sealed             true
    Total Shares       0
    Threshold          0
    Unseal Progress    0/0
    Unseal Nonce       n/a
    Version            1.15.6
    Build Date         n/a
    Storage Type       raft
    HA Enabled         true

Now you can create an unseal key. For this tutorial we will only use a single key but in a production environment you will want to require more than 1 key-share to unseal Vault.  
Run the following command to unseal Vault and export the unseal token and root key.

.. code:: bash

    key_init=$(vault operator init -key-shares=1 -key-threshold=1); echo "$key_init"
    export VAULT_TOKEN=$(echo "$key_init" | sed -n -e 's/.*Root Token: //p'); echo "Root Token = $VAULT_TOKEN"
    export UNSEAL_KEY=$(echo "$key_init" | sed -n -e 's/.*Unseal Key 1: //p'); echo "Unseal Key = $UNSEAL_KEY"
    vault operator unseal "$UNSEAL_KEY"
    juju run vault/leader authorize-charm token="$VAULT_TOKEN"

Now run ``juju status`` again and confirm your Vault unit is in an active state.

Finally, save the root token and unseal key to disk for use later. The unseal key is especially important if your PC is restarted as Vault will become resealed and the unseal key will be needed again.

.. code:: bash

    echo $UNSEAL_KEY > vault_unseal_key.txt
    echo $VAULT_TOKEN > vault_token.txt

We are now ready to move onto the next step.

Configure JIMM
--------------

Nearing the end, we will configure JIMM. Here we will configure required config parameters with an explanation of what they do.

Run the following commands:

.. code:: bash
    
    # The UUID value is used internally to represent the JIMM controller in OpenFGA relations/tuples.
    # Changes to the UUID value after deployment will likely result in broken permissions.
    # Use a randomly generated UUID.
    juju config jimm uuid=3f4d142b-732e-4e99-80e7-5899b7e67e59
    # The address to reach JIMM, this will configure ingress and is also used for OAuth flows/redirects.
    juju config jimm dns-name=test-jimm.localhost
    # A private and public key for macaroon based authentication with Juju controllers.
    juju config jimm public-key="<public-key>"
    juju config jimm private-key="<private-key>"

Optionally, if you have deployed Juju Dashboard, you can configure JIMM to enable browser flow for authentication:

.. code:: bash

    juju config jimm juju-dashboard-location="<juju-dashboard-url>"

.. note::
    However, in absence of a Juju Dashboard, you can still enable OAuth browser authentication flow by setting this parameter to any valid URL. For example:

    .. code:: bash

        juju config jimm juju-dashboard-location="http://test-jimm.localhost/auth/whoami"

Note that the public and private key pairs must be generated by the `go macaroon bakery repository <https://github.com/go-macaroon-bakery/macaroon-bakery>`__.
To do this briefly run the following command, ensuring you have the ``go`` tool installed:

.. code:: bash

    go run github.com/go-macaroon-bakery/macaroon-bakery/cmd/bakery-keygen/v3@latest

This should return a private and public key pair as below which can be used to configure JIMM.
These values are only used internally between JIMM and Juju controllers.

.. code:: json

    {
        "public": "<public-key>",
        "private": "<private-key>"
    }

At this point you can run ``juju status`` and you should observe JIMM is active.  
Navigate to ``http://test-jimm.localhost/debug/info`` to verify your JIMM deployment.

Finally we will obtain the ca-certificate generated to ensure that we can connect to JIMM with HTTPS. 
This is necessary for the Juju CLI to work properly

.. code:: bash
    juju run jimm-cert/0 get-ca-certificate --quiet | yq .ca-certificate | sudo tee /usr/local/share/ca-certificates/jimm-test.crt
    sudo update-ca-certificates

Verify that you can securely connect to JIMM with the following command:

.. code:: bash
    curl https://jimm.test.localhost/debug/info

Using Your JIMM Deployment
--------------------------

Now that you have JIMM running you can browse our additional guides to start adding controllers and workloads.

- Setup your initial JIMM admin and configure permissions.
- Learn how to add a new controller to JIMM.
- Learn how to migrate models from existing controllers to JIMM.
- Understand the difference between the ``juju``, ``jaas`` and ``jimmctl`` CLI tools.

Cleanup
-------

To remove the Juju controller you initially created and all models with associated applications, run the following command:

.. code::

    juju destroy-controller --destroy-all-models --destroy-storage --no-prompt jimm-demo-controller

And to cleanup the Multipass VM if one was used:

.. code::

    multipass delete --purge jimm-deploy
