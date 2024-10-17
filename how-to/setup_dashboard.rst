JAAS: Setup Juju dashboard
==========================

Juju Dashboard aims to expose Juju environments, providing at-scale management, status 
and collaboration features not found in the Juju CLI. 

.. hint::
    To explore Juju Dashboard feature you can go `here <https://juju.is/docs/juju/the-juju-dashboard>`_


Prerequisites
-------------

For this how-to you will need the following:

- A running JAAS environment, see :doc:`our tutorial <../tutorial/deploy_jaas_microk8s>`.

Deploy Juju Dashboard
---------------------
First deploy the Juju Dashboard charm.

.. code:: bash
    juju switch <model_where_jimm_is>
    juju deploy juju-dashboard-k8s dashboard
    juju integrate dashboard jimm-app

Then you need to expose your dashboard through an ingress.
You can follow `this guide <setup_ingress_with_tls>`_.

.. code:: bash
    juju deploy nginx-ingress-integrator dashboard-ingress
    juju integrate dashboard dashboard-ingress
    juju config dashboard-ingress service-hostname="<https://hostname>""

You would access the dashboard from ``https://hostname``

Now you need to configure JIMM to accept requests coming from ``https://hostname``.
.. code:: bash
    juju config jimm-app cors-allowed-origins="https://hostname"
    juju config jimm-app juju-dashboard-location="https://hostname"

Now go to ``https://hostname``, sign in through the identity provider you setup during JAAS deployment, and you 
are in the dashboard.