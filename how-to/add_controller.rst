JAAS: Add controller to JIMM
============================


Introduction
------------

JIMM gives a centralised view of all models in the system. However the work of managing 
the models is delegated to a set of Juju controllers deployed in various clouds
and regions.

These Juju controllers must be deployed with some specific options to ensure they work
correctly in the JAAS system. This document discusses how to bootstrap a Juju controller
such that it will work correctly in a JAAS system.

In this how-to we will show how to add Juju controllers deployed in both MicroK8s and LXD to 
a JIMM controller.

Prerequisites
-------------

For this tutorial you will need the following:

- Basic knowledge of Juju
- A JIMM controller deployed in MicroK8s, see :doc:`the tutorial <../tutorial/deploy_jaas_microk8s>`.
- Administrator permission on the JIMM controller, see :doc:`bootstrapping permissions <./bootstrap_permissions>`.


Prelude
-------

In order for a Juju controller to trust a JIMM controller, the ``login-token-refresh-url`` config option must 
be specified when bootstrapping the Juju controller.

This config option is set to a specific URL path that serves JIMM's public key, which is used to verify signed 
requests when they reach the Juju controller.

MicroK8s Controller
-------------------

The following section provides guidance on how to connect a controller bootstrapped on MicroK8s to your JIMM running in MicroK8s.

We will name this controller ``workload-microk8s`` as it will be running our workloads
as opposed to our original controller which only deploys JAAS.

.. code:: bash

    juju bootstrap microk8s workload-microk8s --config login-token-refresh-url=http://jimm-endpoints.jimm.svc.cluster.local:8080/.well-known/jwks.json

.. note::
    
    The hostname comes from Kubernetes DNS functionality. See more `here <https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#a-aaaa-records>`__. 

Once this process is complete we will switch back to JIMM and add the controller to JIMM.

.. code:: bash

    juju switch jimm
    jimmctl controller-info workload-microk8s ~/snap/jimmctl/common/k8s-controller-info.yaml --local --tls-hostname juju-apiserver
    jimmctl add-controller ~/snap/jimmctl/common/k8s-controller-info.yaml

The ``controller-info`` command creates a YAML file with information about the controller and with the add-controller command we
pass this information to JIMM, which then connects to the new controller.

.. note::

    | A Juju server's default certificate contains a `SAN <https://en.wikipedia.org/wiki/Subject_Alternative_Name>`__ for the name ``juju-apiserver``.
    | This is why we specify the ``--tls-hostname juju-apiserver`` flag when running the controller-info command.


The use of the ``--local`` flag avoids the need to provide a public DNS address and ``--tls-hostname`` provides the expected
hostname used in TLS, a useful way of handling TLS issues during local development. These config options are normally not needed
in a production environment.


LXD Controller
--------------

The following section provides guidance on how to connect a controller bootstrapped on LXD to your JIMM running in MicroK8s.

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

- Create a Cloud-init template, Cloud-init provisions the LXD container that Juju will use.
- The Cloud-init script will create an entry in ``/etc/hosts`` to point ``test-jimm.localhost`` to the LXD bridge address in order to route this request to your host network.
- The Cloud-init script will add the CA cert in ``/usr/local/share/ca-certificates/jimm-test.crt`` to the machine. If you've placed JIMM's CA cert elsewhere, please update this file location.
- Finally the bash script will bootstrap Juju and configure it to communicate with JIMM.

Next, it is helpful to understand that we are traversing from the isolated network of the container through to 
the host's network and to the LXD container where our Juju controller resides. This is possible thanks to the ``host-access``
add-on in MicroK8s which allows containers to access the host network through a fixed IP address.

Connect our new controller to JIMM.

.. code:: bash

    juju switch jimm
    jimmctl controller-info workload-lxd ~/snap/jimmctl/common/lxd-controller-info.yaml --local --tls-hostname juju-apiserver
    jimmctl add-controller ~/snap/jimmctl/common/lxd-controller-info.yaml
