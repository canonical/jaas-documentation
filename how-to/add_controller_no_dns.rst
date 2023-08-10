JAAS: Add controller to JIMM without DNS
========================================

Introduction
------------

The :doc:`add_controller` doc is a full guide on how to setup a controller, provide it with a load balancer front-end and use the load-balancer to terminate TLS connections.
This guide provides a simplified setup that shows how to get a controller up and running with JIMM without the need for a load-balancer and a DNS address.

This guide is intended for testing and development purposes only as the Juju controller cannot be created in an HA (high availability) setup.

Prerequisites
-------------

For this tutorial you will need the following:

- AWS credentials
- Basic knowledge of juju
- Admin access to a JIMM controller (see this tutorial). For this tutorial we will assume this JIMM is located at jimm.canonical.example.com

Deploy controller
-----------------

1. First we will prepare some parameters for the new controller and export environment variables that we will use in this tutorial. 

    The **controller name** is the name given to the controller both on the local system and within JIMM. For visibility this often includes the name of the JAAS system, the cloud, the cloud-region and some kind of unique identifier, for example jaas-aws-us-east-1-001. 

    The **cloud** is the cloud in which the controller is being bootstrapped. 

    The **cloud region** is the region in which the controller is being bootstrapped. 

    The **Candid URL** is the URL of the candid server that is providing the centralized identity service for the JAAS system. 

    The **JIMM URL** is the URL of the JIMM system providing the JAAS service.

    +----------------------+----------------------+
    | Parameter            | Environment variable |
    +======================+======================+
    | Controller nam       | $NAME                |
    +----------------------+----------------------+
    | Cloud                | $CLOUD               |
    +----------------------+----------------------+
    | Cloud Region         | $REGION              |
    +----------------------+----------------------+
    | Candid URL           | $CANDID              |
    +----------------------+----------------------+
    | JIMM URL             | $JIMM                |
    +----------------------+----------------------+


2. Now we are ready to bootstrap a controller. Please note the constraints here are the ones used for production JAAS services and should be suitable for most loads. If it is anticipated that the JAAS system will have a different model profile then we encourage you to determine the appropriate constraints for your system: 

    ``juju bootstrap --bootstrap-constraints="root-disk=50G cores=8 mem=8G" --config login-token-refresh-url=https://$JIMM/.well-known/jwks.json --config allow-model-access=true $CLOUD/$REGION $NAME``

3. The we switch to the controller model: 

    ``juju switch controller``

4.  Install the jaas snap that you download here (note that this will eventually change to be accessible from https://snapcraft.io/jimmctl):

    https://drive.google.com/file/d/1LiOvVpVQ13V3x3l2PhgS2fTHDUtCEe7p/view?usp=sharing 

5. To add the bootstrapped controller to JIMM we need to create a controller-information document. To do this, run the following command:
    The "--local" flag allows you to skip providing the DNS address of your Juju controller.

    ``/snap/jaas/current/bin/jimmctl controller-info --local $NAME $NAME.yaml``

6. Now we can switch to JIMM: 
    
    ``juju switch $JIMM``

7. And add the controller to JIMM with the command: 
    
    ``/snap/jaas/current/bin/jimmctl add-controller $NAME.yaml``
    
Following these steps you added an AWS controller to your JIMM. You should now be able to add models in AWS: juju add-model test aws
