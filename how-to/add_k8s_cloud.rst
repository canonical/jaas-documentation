JAAS: Add a K8S cloud to JIMM
=============================

Introduction
------------
In this how-to, we will show you how to add a K8S cloud to JIMM
and deploying a model.

Prerequisites
-------------

For this how-to, you will require the following:

- Basic knowledge of Juju
- Have JIMM deployed with admin access
- Have a machine based controller added to JIMM
- Basic knowledge of K8S
- A controller running in the region and subnet 
  (or another subnet given the routing rules allow connectivity) 
  as your K8S deployment, whether that be microk8s, EKS, etc.

Ensure kube config is set correctly
-----------------------------------

Firstly we'll need a local kube config correctly configured
with the K8S cluster you wish to add to JIMM.

The default location for this file can be found here:

``cat ~/.kube/config``

In this `contexts` object, locate your desired context and remember
its "name" field.

Updating JIMM to be aware of this cloud
---------------------------------------

Next we'll add the cloud by context to JIMM.

Firstly let us switch to the JIMM controller (note, this is not the controller JIMM is running on but JIMM itself), run:
``juju switch <your jimm controller>``

Add your K8S cloud using the following command:
``juju add-k8s --context-name <context field from ~/.kube/config> --cloud=<cloud name> --region=<region name> <your k8s cloud name>``

.. note::
    --cloud (the literal cloud name) and --region (the cloud region) are used by JIMM
    to determine which controller to add this cloud to. For example,
    if you have a controller running on aws/eu-west-2, then JIMM will add this K8S cloud to that
    underlying controller.

Presuming you are running on EKS, your context name is operatorinc and your region is eu-west-2, your command may look like:
``juju add-k8s --context-name operatorinc --cloud=aws --region=eu-west-2 k8s-operatorinc``

JIMM will now take these credentials, and add them to the controller within that cloud and region.

You can check if the cloud has been uploaded to the controller managed by JIMM via:
``juju clouds --controller <your controller name>``

Finally, we need to update JIMM with the credentials for your user to add-models.
As you're an admin, no access grants are required:
``juju add-credential <your k8s cloud name>``

This will take you through an interactive input terminal, for each section
 please do the following:

- Credential name: name it whatever you like.
- Region: please use the region your cloud resides on. Typically a tooltip
    will state the region for you.
- Auth Types: select OAuth2.0 and retrieve your  access token and user for 
    your cluster from `~/.kube/config`, this is located in the user section.
    (note, this only applies to users created in k8s using oauth2 otherwise, if you have a basic user [with
a user and password] please select userpass.)
- RBAC ID: Skip this field.

Finally, you can now add a K8S model to your JIMM managed controller! Try:
``juju add-model test <your k8s cloud name>``

Allowing users to create models using the new cloud
---------------------------------------------------
For external users to create models on this new cloud, they will require access.

Firstly, have the user login:
``juju login your.jimm.domain

Ask them for their username, this can be retrieved via:
``juju whoami``

On your JIMM instance where you are the administrator, run:
``juju grant-cloud <username> add-model <your k8s cloud name>``

Lastly, request the user runs the following on the JIMM controller:
``juju add-credential <your k8s cloud name>``

Please note, they will require their own credentials to the cluster and be 
expected to enter them.