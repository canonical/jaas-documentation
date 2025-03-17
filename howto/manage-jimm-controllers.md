(manage-jimm-controllers)=
# Manage JIMM controllers

(deploy-a-jimm-controller)=
## Deploy a JIMM controller

## Integrate a JIMM controller with the Canonical Observability Stack

This document shows how to integrate the different components of JAAS with the
[Canonical Observability Stack][cos] to enable pre-configured dashboards and alerting rules.

The Canonical Observability Stack is a Juju bundle that includes a series of
open source observability applications and related automation.
For the complete list of components in COS, read the
[Component List](https://charmhub.io/topics/canonical-observability-stack/editions/lite).

### Prerequisites

- A running `COS-Lite` bundle.
  You can follow the [Getting started on MicroK8s](https://charmhub.io/topics/canonical-observability-stack/tutorials/install-microk8s).
  tutorial to get you started. Make sure to follow the section **Deploy the COS Lite bundle with overlays** section to create offers.
- A running JAAS. Please refer to the deployment {doc}`tutorial<../tutorial/deploy_jaas_microk8s>`.

```{tip}
[Juju offers](https://juju.is/docs/juju/manage-offers) are a way of sharing software as a service between models. Make sure you deploy COS and setup offers so that you can relate to it across models.
```

It is generally recommended to keep the observability stack separate from any observed applications to separate failure domains.
This document assumes that JAAS and the COS bundle are deployed to different models.

This how-to assumes that Vault and PostgreSQL are deployed alongside JIMM and OpenFGA. Depending on your approach, this may not be true.
Additionally this how-to assumes the names of the deployed applications, which might differ in your environment.

### Integration approaches

There are 2 possible  integration approaches depending on your networking / deployment setup:

1. If you are able to send metrics and logs directly to the observability platform components follow
   the Integrate JAAS with COS-Lite section
2. If you prefer using a telemetry collector component follow
   the Integrate JAAS with COS-Lite through Grafana-Agent section

### Integrate JAAS with COS-Lite

#### Grafana integration

Assuming you deployed the COS-Lite bundle in model `cos-model` with user admin, use the following
commands to integrate the JAAS applications by means of an application offer.

```text
juju integrate jimm admin/cos-model.grafana-dashboards
juju integrate openfga admin/cos-model.grafana-dashboards
juju integrate postgresql admin/cos-model.grafana-dashboards
juju integrate vault admin/cos-model.grafana-dashboards
```

#### Loki integration

Assuming you deployed the COS-Lite bundle in model cos-model with user admin, use the following commands
to integrate JAAS by means of an application offer.

```text
juju integrate jimm admin/cos-model.loki-logging
juju integrate openfga admin/cos-model.loki-logging
juju integrate postgresql admin/cos-model.loki-logging
juju integrate vault admin/cos-model.loki-logging
```

#### Prometheus integration

Assuming you deployed the COS-Lite bundle in model `cos-model` with user admin, use the following commands to integrate JAAS by means of an application offer.

```text
juju integrate jimm admin/cos-model.prometheus-scrape
juju integrate openfga admin/cos-model.prometheus-scrape
juju integrate postgresql admin/cos-model.prometheus-scrape
juju integrate vault admin/cos-model.prometheus-scrape
```

### Integrate JAAS with COS-Lite through Grafana-Agent

You first need to deploy the [Grafana-Agent operator](https://charmhub.io/grafana-agent-k8s), which is a telemetry collector used
to aggregate and push information to the COS-lite bundle.

```{tip}
Note that you may perform some relations directly with the COS applications. E.g. the Grafana relation shares any dashboards from the charm to Grafana. This relation should be done as described in the previous section.
```

To deploy Grafana-Agent run:

```text
juju deploy grafana-agent-k8s --channel latest/stable --trust
```

#### Forward Prometheus metrics

Integrate Grafana-Agent with JAAS by running the following commands:

```text
juju integrate grafana-agent-k8s jimm:metrics-endpoint
juju integrate grafana-agent-k8s openfga:metrics-endpoint
juju integrate grafana-agent-k8s postgresql:metrics-endpoint
juju integrate grafana-agent-k8s vault:metrics-endpoint
```

#### Forward Loki metrics

Integrate Grafana-Agent with JAAS by running the following commands:

```text
juju integrate grafana-agent-k8s jimm:logging
juju integrate grafana-agent-k8s openfga:log-proxy
juju integrate grafana-agent-k8s postgresql:logging
juju integrate grafana-agent-k8s vault:logging
```

#### Integrate Grafana-Agent with COS-Lite

Assuming you deployed the COS-Lite bundle in model `cos-model` with user admin,
use this command to integrate the Grafana-Agent with Prometheus by means of an application offer.

```text
juju integrate grafana-agent-k8s admin/cos-model.prometheus-receive-remote-write
```

Assuming you deployed the COS-Lite bundle in model `cos-model` with user admin,
use this command to integrate the Grafana-Agent with Loki by means of an application offer.

```text
juju integrate grafana-agent-k8s admin/cos-model.loki-logging
```

### Access the dashboards

You can get the Grafana IP address with the [`juju status`](https://juju.is/docs/juju/status) command.
The default port for the Grafana HTTP server is 3000.

The default credentials are:

- **Username**: admin
- **Password**: you can get the password with the juju action [`get-admin-password`](https://charmhub.io/grafana-k8s/actions).

Once in, you will see a vertical menu bar on the left side of the page.
You will find the available alerts by clicking on the Alerting menu.
You will find the available dashboards by clicking on the Dashboards menu

[canonical]: https://canonical.com/
[iam]: https://charmhub.io/topics/canonical-identity-platform
[cos]: https://charmhub.io/topics/canonical-observability-stack


## Equip a JIMM controller with TLS ingress

The NGINX Ingress Integrator is a charm responsible for creating Kubernetes ingress rules,
these rules can be hardened via TLS and the charm provides a means to do so. See [here](https://charmhub.io/nginx-ingress-integrator).

Our LEGO charms provide certificates for charms from a desired ACME server and can be integrated
with the integrator to enable TLS at the ingress level. See [here](https://charmhub.io/httprequest-lego-k8s).

You will require a domain that your ACME is aware of and an NGINX ingress controller installed
on your Kubernetes cluster.

With JAAS deployed, you can deploy both LEGO and the integrator, and integrate your LEGO charm deployment
to your ingress integrator, and then the ingress integrator to JIMM to enable TLS ingress for your deployment.

## Integrate a JIMM controller with the Juju dashboard

Juju dashboard is a web UI that is intended to supplement the CLI experience with aggregate views and at a glance health checks.

This how-to provides you with instructions on how to setup Juju Dashboard for your JAAS deployment.

```{tip}
To explore Juju Dashboard features you can go [here](https://juju.is/docs/juju/the-juju-dashboard).
```

### Prerequisites

For this how-to you will need the following:

- A running JAAS environment, see {doc}`our tutorial <../tutorial/deploy_jaas_microk8s>`.

### Deploy Juju Dashboard

First deploy the Juju Dashboard charm.

```text
juju switch <model_where_jimm_is>
juju deploy juju-dashboard-k8s dashboard
juju integrate dashboard jimm-app
```

Then you need to expose your dashboard through an ingress.

```{tip}
You can follow {doc}`this guide <./setup_ingress_with_tls>` to add TLS to your ingress.
```

```text
juju deploy nginx-ingress-integrator dashboard-ingress
juju integrate dashboard dashboard-ingress
juju config dashboard-ingress service-hostname="<https://hostname>""
```

You will visit your dashboard at `https://hostname`.

Now you need to configure JIMM to accept requests coming from `https://hostname`.

```text
juju config jimm-app cors-allowed-origins="https://hostname"
juju config jimm-app juju-dashboard-location="https://hostname"
```

Now go to `https://hostname`, sign in through the identity provider you setup during JAAS deployment, and you
are in the dashboard.