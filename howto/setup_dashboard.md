# JAAS: Setup Juju dashboard

Juju dashboard is a web UI that is intended to supplement the CLI experience with aggregate views and at a glance health checks.

This how-to provides you with instructions on how to setup Juju Dashboard for your JAAS deployment.

```{tip}
To explore Juju Dashboard features you can go [here](https://juju.is/docs/juju/the-juju-dashboard).
```


## Prerequisites

For this how-to you will need the following:

- A running JAAS environment, see {doc}`our tutorial <../tutorial/deploy_jaas_microk8s>`.

## Deploy Juju Dashboard

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