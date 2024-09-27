Integrate With Canonical Observability Stack (COS)
==================================================

This document shows how to integrate the different components of JAAS with the 
`Canonical Observability Stack`_ to enable pre-configured dashboards and alerting rules.

The Canonical Observability Stack `COS-Lite`_ is a Juju bundle that includes a series of
open source observability applications and related automation. 
For the complete list of components in COS, read the 
`Component List <https://charmhub.io/topics/canonical-observability-stack/editions/lite>`__.

Prerequisites
-------------

- A running `COS-Lite`_ bundle. 
  You can follow the `Getting started on MicroK8s <https://charmhub.io/topics/canonical-observability-stack/tutorials/install-microk8s>`__
  tutorial to get you started. Make sure to follow the section **Deploy the COS Lite bundle with overlays** section to create offers.
- A running JAAS. Please refer to the deployment :doc:`tutorial<../tutorial/deploy_jaas_microk8s>`.

.. hint::
    `Juju offers <https://juju.is/docs/juju/manage-offers>`__ are a way of sharing software as a service between models.
    Make sure you deploy COS and setup offers so that you can relate to it across models.
 
It is generally recommended to keep the observability stack separate from any observed applications to separate failure domains. 
This document assumes that JAAS and the COS bundle are deployed to different models.

This how-to assumes that Vault and PostgreSQL are deployed alongside JIMM and OpenFGA. Depending on your approach, this may not be true.
Additionally this how-to assumes the names of the deployed applications, which might differ in your environment.

Integration approaches
----------------------

There are 2 possible  integration approaches depending on your networking / deployment setup:

1. If you are able to send metrics and logs directly to the observability platform components follow 
   the `Integrate JAAS with COS-Lite`_ section
2. If you prefer using a telemetry collector component follow 
   the `Integrate JAAS with COS-Lite through Grafana-Agent`_ section

Integrate JAAS with COS-Lite 
----------------------------

Grafana integration 
^^^^^^^^^^^^^^^^^^^

Assuming you deployed the COS-Lite bundle in model ``cos-model`` with user admin, use the following 
commands to integrate the JAAS applications by means of an application offer.

.. code:: bash

    juju integrate jimm admin/cos-model.grafana-dashboards
    juju integrate openfga admin/cos-model.grafana-dashboards
    juju integrate postgresql admin/cos-model.grafana-dashboards
    juju integrate vault admin/cos-model.grafana-dashboards

Loki integration 
^^^^^^^^^^^^^^^^
Assuming you deployed the COS-Lite bundle in model cos-model with user admin, use the following commands
to integrate JAAS by means of an application offer.

.. code:: bash

    juju integrate jimm admin/cos-model.loki-logging
    juju integrate openfga admin/cos-model.loki-logging
    juju integrate postgresql admin/cos-model.loki-logging
    juju integrate vault admin/cos-model.loki-logging

Prometheus integration 
^^^^^^^^^^^^^^^^^^^^^^
Assuming you deployed the COS-Lite bundle in model ``cos-model`` with user admin, use the following commands
to integrate JAAS by means of an application offer.

.. code:: bash

    juju integrate jimm admin/cos-model.prometheus-scrape
    juju integrate openfga admin/cos-model.prometheus-scrape
    juju integrate postgresql admin/cos-model.prometheus-scrape
    juju integrate vault admin/cos-model.prometheus-scrape

Integrate JAAS with COS-Lite through Grafana-Agent
--------------------------------------------------

You first need to deploy the `Grafana-Agent operator <https://charmhub.io/grafana-agent-k8s>`__, which is a telemetry collector used
to aggregate and push information to the COS-lite bundle.

.. hint::
    Note that you may perform some relations directly with the COS applications. E.g. the Grafana relation shares any dashboards
    from the charm to Grafana. This relation should be done as described in the previous section.

To deploy Grafana-Agent run:

.. code:: bash

    juju deploy grafana-agent-k8s --channel latest/stable --trust

Forward Prometheus metrics
^^^^^^^^^^^^^^^^^^^^^^^^^^
Integrate Grafana-Agent with JAAS by running the following commands:

.. code:: bash

    juju integrate grafana-agent-k8s jimm:metrics-endpoint
    juju integrate grafana-agent-k8s openfga:metrics-endpoint
    juju integrate grafana-agent-k8s postgresql:metrics-endpoint
    juju integrate grafana-agent-k8s vault:metrics-endpoint

Forward Loki metrics
^^^^^^^^^^^^^^^^^^^^
Integrate Grafana-Agent with JAAS by running the following commands:

.. code:: bash

    juju integrate grafana-agent-k8s jimm:logging
    juju integrate grafana-agent-k8s openfga:log-proxy
    juju integrate grafana-agent-k8s postgresql:logging
    juju integrate grafana-agent-k8s vault:logging

Integrate Grafana-Agent with COS-Lite
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Assuming you deployed the COS-Lite bundle in model ``cos-model`` with user admin,
use this command to integrate the Grafana-Agent with Prometheus by means of an application offer.

.. code:: bash

    juju integrate grafana-agent-k8s admin/cos-model.prometheus-receive-remote-write

Assuming you deployed the COS-Lite bundle in model ``cos-model`` with user admin, 
use this command to integrate the Grafana-Agent with Loki by means of an application offer.

.. code:: bash

    juju integrate grafana-agent-k8s admin/cos-model.loki-logging

Access the dashboards
---------------------
You can get the Grafana IP address with the `juju status <https://juju.is/docs/juju/status>`__ command. 
The default port for the Grafana HTTP server is 3000.

The default credentials are:

- **Username**: admin 
- **Password**: you can get the password with the juju action `get-admin-password <https://charmhub.io/grafana-k8s/actions>`__.

| Once in, you will see a vertical menu bar on the left side of the page.
| You will find the available alerts by clicking on the Alerting menu.  
| You will find the available dashboards by clicking on the Dashboards menu