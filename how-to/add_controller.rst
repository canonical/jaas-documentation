JAAS: Add controller to JIMM
============================


Introduction
------------

JIMM gives a centralised view of all models in the system. However the work of managing 
the models is delegated to a set of juju controllers deployed in various clouds
and regions.

These juju controllers must be deployed with some specific options to ensure they work
correctly in the JAAS system. This document discusses how to bootstrap a juju controller
such that it will work correctly in a JAAS system.

In this how-to we will show how to add Juju controllers deployed in both MicroK8s and LXD to 
a JIMM controller.

Prerequisites
-------------

For this tutorial you will need the following:

- Basic knowledge of juju
- A JIMM controller deployed in MicroK8s, see :doc:`the tutorial <../tutorial/deploy_jaas_microk8s>`.
- Administrator permission on the JIMM controller, see ``todo``.


Prelude
-------

In order for a Juju controller to trust a JIMM controller, a specific config option must be set known
as the ``login-token-refresh-url``.

This config option is set to a specific URL path on the JIMM controller that serves JIMM's public key.
Requests from JIMM to the Juju controller are signed, and verified when they reach the Juju.

MicroK8s Controller
-------------------

Bootstrap a new Juju controller in MicroK8s. We will name this controller ``workload-microk8s`` as it will be running our workloads
as opposed to our original controller which only deploys JAAS.

.. code:: bash

    juju bootstrap microk8s workload-microk8s --config login-token-refresh-url=http://jimm-endpoints.jimm.svc.cluster.local:8080/.well-known/jwks.json

Once this process is complete we will switch back to JIMM and add the controller to JIMM.

.. code:: bash

    juju switch jimm
    jimmctl controller-info workload-microk8s ~/snap/jimmctl/common/k8s-controller-info.yaml --local --tls-hostname juju-apiserver
    jimmctl add-controller ~/snap/jimmctl/common/k8s-controller-info.yaml

The above commands create a YAML file with information about the controller and passes this information to JIMM.

.. note::

    A Juju server's default certificate contains a `SAN <https://en.wikipedia.org/wiki/Subject_Alternative_Name>`__ for the name ``juju-apiserver``.

The use of the ``--local`` flag avoids the need to provide a public DNS address and ``--tls-hostname`` provides the expected
hostname used in TLS, a useful way of handling TLS issues during local development. These config options are normally not needed
in a production environment.


LXD Controller
--------------

The following section provides guidance on how to connect a controller bootstrapped on LXD to your JIMM running in MicroK8s.

The steps will be similar to those for adding a MicroK8s hosted controller but because we are traversing from the isolated network
of the container through to LXD's network, there will be additional steps.

Run the following commands to bootstrap a LXD based controller:

.. code:: bash

    CLOUDINIT_FILE="cloudinit-tweak.temp.yaml"
    CONTROLLER_NAME="workload-lxd"
    CLOUDINIT_TEMPLATE=$'cloudinit-userdata: |
    preruncmd:
        - echo "%s    test-jimm.localhost" >> /etc/hosts
    ca-certs:
        trusted:
        - |\n%s'
    printf "$CLOUDINIT_TEMPLATE" "$(lxc network get lxdbr0 ipv4.address | cut -f1 -d/)" "$(cat /usr/local/share/ca-certificates/jimm-test.crt | sed -e 's/^/\t  /')" > "${CLOUDINIT_FILE}"
    juju bootstrap lxd "${CONTROLLER_NAME}" --config "${CLOUDINIT_FILE}" --config login-token-refresh-url=https://test-jimm.localhost/.well-known/jwks.json --debug 

The set of commands will do the following:

- Create a cloud-init template, cloud-init provisions the LXD container that Juju will use.
- The cloud-init script will create an entry in ``/etc/hosts`` to point ``test-jimm.localhost`` to the LXD bridge address in order to route this request to your host network.
- The cloud-init script will add the CA cert in ``/usr/local/share/ca-certificates/jimm-test.crt`` to the machine. If you've placed JIMM's CA cert elsewhere, please update this file location.
- Finally the bash script will bootstrap Juju and configure it to communicate with JIMM.

Next, we will create a network relay to forward traffic from our host network through to the Juju server running in a LXC container.

.. note::
    The network relay relies on the ``socat`` application running continuously in the background.  
    The application will need to be run again between system reboots.

.. code:: bash

    JUJU_ADDRESS=$(juju show-controller workload-lxd --format yaml | yq .workload-lxd.details.api-endpoints.[0])
    socat tcp-listen:8001,reuseaddr,fork tcp:$JUJU_ADDRESS

To test the relay is working run the following command which should return a HTTP 400 response code.

.. code:: bash

    curl https://localhost:8001 -k -I

Finally, we can connect our new controller to JIMM.

.. code:: bash

    juju switch jimm
    jimmctl controller-info workload-lxd ~/snap/jimmctl/common/lxd-controller-info.yaml --local --tls-hostname juju-apiserver
    jimmctl add-controller ~/snap/jimmctl/common/lxd-controller-info.yaml
