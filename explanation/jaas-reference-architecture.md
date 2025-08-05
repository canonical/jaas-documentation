(reference-architecture)=
# Reference architecture
## Introduction

### Key objectives

| Requirement| Solution capabilities |
|-|-|
| Automation | JAAS supports the Terraform Provider for Juju and exposes the same API as a Juju controller. For the Terraform Provider for Juju it must support authentication using valid service accounts, which are authorised to perform required operations (e.g. add models, add permissions). |
| Authentication - OAuth2/OIDC | JAAS supports the OAuth2/OIDC authentication via the Canonical Identity Platform. |
| Authorisation - ReBAC | JAAS supports ReBAC via OpenFGA. |
| Authorisation - RBAC | JAAS supports Juju’s default RBAC. |
| Juju features - `juju` CLI | JAAS supports the `juju` CLI as it exposes the same API as a Juju controller. |
| Juju features - Terraform Provider | JAAS supports the Terraform Provider for Juju, but requires service account authentication via the Canonical Identity Platform. |

### Technologies

The table below lists the main technologies used in the reference architecture with links to their corresponding documentation.

| Technology                     | Documentation                                                                 |
|-------------------------------|---------------------------------------------------------------------------------|
| JAAS                          | [JAAS docs](https://documentation.ubuntu.com/jaas/v3/) |
| Juju                          | [Juju docs](https://documentation.ubuntu.com/juju/3.6/)                         |
| Charms                        | [Charmcraft docs](https://canonical-charmcraft.readthedocs-hosted.com/), [Ops docs](https://documentation.ubuntu.com/ops/) |
| Canonical Kubernetes          | [Canonical Kubernetes docs](https://documentation.ubuntu.com/canonical-kubernetes/) |
| Canonical Observability Stack (COS) | [Canonical Observability Stack docs](https://documentation.ubuntu.com/observability/) |
| Canonical Identity Platform (CIdP)  | [Canonical Identity Platform docs](https://charmhub.io/topics/canonical-identity-platform)     |
| PostgreSQL                    | [PostgreSQL docs](https://www.postgresql.org/docs/)                             |
| OpenFGA                       | [OpenFGA docs](https://openfga.dev/docs/fga)                                     |
| Vault                         | [Vault docs](https://developer.hashicorp.com/vault/docs)                        |
| Terraform                     | [Terraform docs](https://developer.hashicorp.com/terraform/docs)                |
| Terraform Provider for Juju   | [Terraform Provider for Juju docs](https://documentation.ubuntu.com/terraform-provider-juju/) |

## Architecture component details

(reference-architecture-overview)=
### Overview

The image below shows an overview of a JAAS deployment.

At its core is the charmed JIMM application deployed with Juju using the reference Terraform module along with the application offers it needs:

- PostgreSQL, to store all stateful data;
- Vault, to store controller and cloud credentials;
- OpenFGA, for Relationship Based Access Control (ReBAC) authorization;
- Canonical Identity Platform (CIdP), for authentication;

Each component must be deployed according to their respective reference architecture and related to JIMM via cross-model relations.

In addition, JIMM should be related to the Canonical Observability Stack (COS) for observability.

```{figure} reference-architecture-components.svg
   :alt: JAAS architecture components -- deployment view
   :width: 1000px
  _JAAS architecture components -- deployment view._
```

### Artifacts

JAAS is distributed in the form of three types of artifacts: snaps, rocks, and charms.

#### Snaps

Snaps are application container images that are designed to be strictly confined and secure by default ([see more](https://snapcraft.io/docs)). JAAS uses snaps to deliver CLI tools used for the management of JAAS and for authorization management. For this purpose we provide:
- [the `jaas` snap](https://snapcraft.io/jaas), a `juju` CLI plugin which requires the `juju` CLI to be installed.

#### Charms

Charms are software operators -- software that contains all of the instructions necessary for deploying, configuring, scaling, integrating, etc., an application ({external+juju:ref}`see more <charm>`). JAAS uses a Kubernetes charm for the purpose of deploying and operating JIMM, which is provided as a resource in the form of a rock:
- [the `juju-jimm-k8s` charm](https://charmhub.io/juju-jimm-k8s) - must be deployed from channel 3/stable for Juju 3.X

#### Rocks

Rocks are OCI-compliant container images that are built to be secure and stable by design ([see more](https://documentation.ubuntu.com/rockcraft/stable/explanation/rocks/)). JAAS uses rocks for the purpose of deploying the JIMM service. The JIMM rock is provided along with the `juju-jimm-k8s` charm in the form of a resource attached to specific revisions of the charm.

- [the `juju-jimm-k8s` charm resources](https://charmhub.io/juju-jimm-k8s/resources/jimm-image)

(reference-architecture-software-versions)=
### Software versions

The following table lists specific versions of software artifacts. First is the version of JIMM that is used in the reference architecture, others are versions of dependencies  that we recommend for integration with JIMM.

| Component                    |   Version        |
|------------------------------|------------------|
| JIMM / `juju-jimm-k8s` charm             | Revision 79      |
| JIMM / `jaas` snap             | v3.2.9           |
| Juju                         | 3.6.8            |
| PostgreSQL / machine charm   | Revision 553     |
| PostgreSQL / K8s charm       | Revision 495     |
| OpenFGA / K8s charm          | Revision 128     |
| Vault / K8s charm            | Revision 323     |
| Vault / machine charm        | Revision 387     |
| CIdP / Hydra / machine charm  | Revision 362     |
| COS / Grafana / K8s charm    | Revision 151     |
| COS / Loki / K8s charm       | Revision 199     |
| COS / Prometheus / K8s charm | Revision 247     |

## Pre-deployment readiness

This section describes the necessary prerequisites for deploying JAAS.

### Juju controller

JAAS deploy requires a Juju 3.6.5 controller (root controller) that must  be used to provision a Kubernetes cluster and deploy dependencies (see {ref}`reference-architecture-overview`).

### Kubernetes cluster

JAAS deploy requires a Kubernetes cluster (e.g., [Canonical Kubernetes](https://documentation.ubuntu.com/canonical-kubernetes/)). Deploying/provisioning the cluster is outside of the scope of this document, but it should be deployed in accordance with its reference architecture and should be of the latest stable version. This document assumes the Kubernetes cluster is added as a cloud to the root controller and the user running the deploy has sufficient permission to add models and deploy charms in this cloud.


### Network Time Protocol (NTP)

All nodes of the Kubernetes cluster should be able to access an appropriate NTP server. The Customer should decide whether to set up one locally or use the public pools available from the Network Time Protocol project (http://www.ntp.org/).

Agreed NTP servers to be used are:

- `0.ubuntu.pool.ntp.org`
- `1.ubuntu.pool.ntp.org`
- `2.ubuntu.pool.ntp.org`
- `3.ubuntu.pool.ntp.org`
- `ntp.ubuntu.com`

The nodes of the Kubernetes cluster must be configured to UTC.

### Networking requirements

| Service Name     | Port                   | Description                                                                                                                                                                     |
|------------------|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PostgreSQL       | TCP/5432               | PostgreSQL is used as a data store by JIMM. JIMM must be able to connect to the subnet running PostgreSQL on port 5432.                                                        |
| OpenFGA          | TCP/8080               | OpenFGA is the core system for JIMM authorization. JIMM must be able to connect to it on port 8080.                                                                            |
| Vault            | TCP/8200               | Vault is used to store controller and cloud credentials. JIMM must be able to connect to Vault on port 8200.                                                                   |
| Hydra            | TCP/443, TCP/16433,6433| JIMM must be able to connect to Hydra on port 443 and 16433/6443 (6433 - for Canonical Kubernetes).                                                                                   |
| COS              | TCP/443                | COS Lite is a light-weight observability stack. Grafana agent must be able to connect to Prometheus, Loki, and Grafana to send logs, metrics, and push dashboards to Grafana. |
| Let’s Encrypt    | TCP/443                | To obtain TLS certificates using the Lego charm, the Kubernetes cluster must be able to access Let’s Encrypt servers on port 443.                                                     |
| Juju controllers | TCP/443, TCP/17070     | JIMM must be able to access managed Juju controllers on ports 443 and 17070.                                                                                                   |

Note: Managed Juju controllers must be able to reach:
- JIMM on port 443 (to obtain a valid JWKS)
- Charmhub API

## Security

### TLS termination

JIMM must be able to securely communicate with the components it depends on over the network. This is accomplished using TLS and public-key encryption with a chain of trust up to a shared root Certificate Authority (CA).

For this purpose the [Lego charm](https://charmhub.io/lego) must be used to obtain X509 certificates for deployed services.

All communication between JIMM and Juju controllers happens over TLS-encrypted websockets using self-signed certificates of the Juju controller.

### Data security

JIMM stores sensitive information, such as controller and cloud credentials, in Vault, while it stores the rest of its state in PostgreSQL.

For Vault please follow the security best practices described for the relevant charm:
- [Vault](https://charmhub.io/vault/docs/h-security-in-charmed-vault)
- [Vault-k8s](https://charmhub.io/vault-k8s/docs/h-security-in-charmed-vault)

For PostgreSQL please follow the security best practices described for the relevant charm:
- [PostgreSQL](https://canonical-charmed-postgresql.readthedocs-hosted.com/14/explanation/security/)
- [PostgreSQL-k8s](https://canonical-charmed-postgresql-k8s.readthedocs-hosted.com/14/explanation/security/)

### OS hardening

For OS hardening please follow the [security best practices for Ubuntu Server](https://documentation.ubuntu.com/security/docs/security-features/security-features-overview/).

### Patching

It is recommended that you keep the Kubernetes cluster on which JIMM is deployed updated to the latest available stable version. That way you will receive the latest bug-fixes and security patches.

**Note:** Kubernetes will automatically handle patch releases. This means that the cluster will perform an unattended automatic upgrade between patch versions, e.g. 1.10.7 to 1.10.8. Attended upgrades are only required when you wish to upgrade a minor version, e.g. 1.9.x to 1.10.x.

## Deployment Terraform module

The JIMM Terraform module is stored and versioned in a source code repository (such as Git) as it is essentially infrastructure-as-code. The module can be used to rebuild the current deployment (in the case of failure or migration) or to deploy additional instances on private infrastructure or public clouds.

The JIMM Terraform module is available at [https://github.com/canonical/jimm-terraform-plan](https://github.com/canonical/jimm-terraform-plan).

## Acceptance tests

Following the deployment of JAAS, users should run a set of acceptance tests. The basic set of tests we recommend is:

- verify usage of Juju CLI against JIMM
- verify usage of the Terraform Provider for Juju against JIMM
- verify ReBAC access management

## Engagement details

### Reporting issues

Any issue should be reported at [https://github.com/canonical/jimm/issues](https://github.com/canonical/jimm/issues).

### JIMM releases

New JIMM releases will be announced at [https://github.com/canonical/jimm/releases](https://github.com/canonical/jimm/releases).

### JIMM upgrades

To upgrade JIMM, upgrade the charm revision in your Terraform plan and apply the new plan. The charm will take care of necessary database schema migrations.

An updated version of the table in {ref}`reference-architecture-software-versions` will be published along with release notes.

### Backup and disaster recovery

JIMM’s state is stored in the PostgreSQL to which it is related, so backup and disaster recovery essentially means backing up and restoring PostgreSQL databases.

JIMM’s authorization system depends on data stored in OpenFGA (which also uses PostgreSQL) and any backup and disaster recovery strategy must involve backups of the OpenFGA database.

For disaster and recovery guidelines for PostgreSQL please refer to the documentation of the corresponding charms:
- for the PostgreSQL VM charm:
  - [backup](https://canonical-charmed-postgresql.readthedocs-hosted.com/14/how-to/back-up-and-restore/create-a-backup/index.html)
  - [restore](https://canonical-charmed-postgresql.readthedocs-hosted.com/14/how-to/back-up-and-restore/create-a-backup/index.html)
- for the PostgreSQL K8s charm:
  - [backup](https://canonical-charmed-postgresql-k8s.readthedocs-hosted.com/14/how-to/back-up-and-restore/create-a-backup/index.html)
  - [restore](https://canonical-charmed-postgresql-k8s.readthedocs-hosted.com/14/how-to/back-up-and-restore/restore-a-backup/index.html)

For backup and disaster recovery of the Juju controllers managed by JIMM consult Juju documentation:
- [backup](https://documentation.ubuntu.com/juju/3.6/howto/manage-controllers/#back-up-a-controller)
- [restore](https://documentation.ubuntu.com/juju/3.6/howto/manage-controllers/#restore-a-controller-from-a-backup)

#### Recovery

In case of data loss or outage, restore things as follows:
- Restore PostgreSQL
- Restore OpenFGA
- Restore Vault
- Restore Identity Platform
- Restart JIMM

### Upgrading the Juju version of the managed controllers

When a new stable version of Juju is released, you should upgrade all your managed models to the new version.

For patch version upgrades you can follow Juju’s in-place upgrade procedure to upgrade individual controllers followed by upgrades of individual models. [See more.](https://documentation.ubuntu.com/juju/3.6/howto/manage-your-deployment/#upgrade-your-deployment)

Minor and major version upgrades of Juju controllers are not supported -- instead you must bootstrap a new Juju controller of a specific version, add it to JIMM, and migrate all your existing models to this controller. [See more.](https://documentation.ubuntu.com/jaas/v3/howto/manage-models/#migrate-a-model-within-jaas)

## Key URLs

The following table describes a list of key URLs for a Kubernetes cluster deployment:

| Item                   | URL                                                                                         |
|------------------------|---------------------------------------------------------------------------------------------|
| JIMM Terraform module  | [GitHub | JIMM module](https://github.com/canonical/jimm-k8s-operator/tree/v3/modules/jimm) |
| JAAS documentation     | [JAAS docs](https://canonical-jaas-documentation.readthedocs-hosted.com/en/v3/)            |


## Appendix

### Technologies explained

#### Juju

Juju is a service modeling software implemented as a distributed system. It uses a model description as an input and delivers an environment according to a set of requirements defined in the model. It is also used to manage the lifecycle of services in a given model.

The following diagram illustrates a typical Juju deployment:

```{figure} reference-architecture-technologies-juju.svg
   :alt: JAAS technologies -- Juju
   :width: 1000px
  _JAAS technologies -- Juju._
```

Juju consists of a number of components:

1. Controller node (physical or virtual) which hosts the API and database components of Juju;
  - Controllers include cloud-specific provisioning logic (OpenStack acts as a cloud in this case);
1. Juju client which is used to bootstrap and manage Juju itself and models provisioned via Juju;
1. Machine agents are used to perform post-deployment machine-related tasks that are relevant for Juju operation and report to Juju controller nodes;
1. Unit agents execute application lifecycle events. The concrete application-specific logic is encoded in charms.

#### Charms

Charms encode the operational knowledge about a particular application in a reusable way. The operational knowledge in question is about deployment, scaling, upgrades, post-deployment reconfiguration, setting up software in a highly available configuration, integrations with other applications etc.

An application provider would typically have the best operational knowledge about a given application and charms provide a way to express this knowledge in a more sophisticated way than what deployment-focused automation and regular configuration tooling can do.

Charms rely on Juju to provide them with a platform for distributed and event-driven code execution, data exchange and a data model for common operations. However, the specifics of how to use that platform for a particular application are outside of Juju and are implemented in concrete charms.

The generic approach to operations taken in Juju and charms makes charms an exceptional tool not only in deploying and operating simple applications like load balancers or web servers but also complex applications such as OpenStack or Kubernetes.

#### Canonical Kubernetes

The release cycle of Charmed Kubernetes (CK) is tightly synchronised to the release of upstream Kubernetes. The current version and two prior releases are supported, giving an effective support period of nine months, subject to changes in the upstream release cycle. Up-to-date information about Ubuntu and CK  support periods is available at https://ubuntu.com/about/release-cycle#canonical-kubernetes-release-cycle. The following diagram is a snapshot from the release lifecycle page:

```{figure} reference-architecture-technologies-ck.svg
   :alt: JAAS technologies -- Canonical Kubernetes
   :width: 1000px
  _JAAS technologies -- Canonical Kubernetes._
```

#### Canonical Observability Stack (COS)

The Canonical Observability Stack gathers, processes, visualises, and alerts on telemetry signals generated by workloads running both within, and outside of, Juju.

By leveraging the topology model of Juju to contextualise the data, and charm relations to automate configuration and integration, it provides a low-ops observability suite based on best-in-class, open-source observability tools.

Documentation:
- [https://documentation.ubuntu.com/observability/](https://documentation.ubuntu.com/observability/)

##### Prometheus

The Prometheus charm provides a cluster monitoring solution using Prometheus, which is an open source metric monitoring system and alerting toolkit.

Upstream documentation : [https://prometheus.io/](https://prometheus.io/)

Prometheus charm: [https://charmhub.io/prometheus-k8s](https://charmhub.io/prometheus-k8s)

##### Loki

Loki is an open-source fully-featured logging system. Loki is a horizontally scalable, multi-tenant log aggregation system inspired by Prometheus. It is designed to be very cost effective and easy to operate. It does not index the contents of the logs, but rather the unique set of labels for each log stream.

Upstream documentation: [https://grafana.com/oss/loki/](https://grafana.com/oss/loki/)

##### Alertmanager

Alertmanager receives alerts from supporting applications, such as Prometheus or Loki, then deduplicates, groups and routes them to the configured receiver(s).

Upstream documentation: [https://prometheus.io/docs/alerting/latest/alertmanager/](https://prometheus.io/docs/alerting/latest/alertmanager/)

##### Grafana

The Grafana Operator provides a data visualisation solution using Grafana, an open-source data visualisation toolkit.

Grafana allows you to query, visualise, and alert on metrics, logs, and traces from mixed data sources in configurable dashboards.

The `grafana-k8s` charm provides an interface which can ingest data from a wide array of data sources, with Prometheus and Loki as common inputs, then presents that data on configurable dashboards. It is the primary user-facing entrypoint for the Canonical Observability Stack Lite. See the COS Lite Bundle for more information.

Upstream documentation: [https://grafana.com/](https://grafana.com/)

#### Canonical Identity Platform (CIdP)

The Canonical Identity Platform (C-IdP) is the new authentication and authorization platform developed by Canonical. It is composed of a set of applications with different purposes.

Documentation: [https://charmhub.io/topics/canonical-identity-platform](https://charmhub.io/topics/canonical-identity-platform)

##### Hydra

Ory Hydra is a hardened, OpenID Certified OAuth 2.0 Server and OpenID Connect Provider optimized for low-latency, high throughput, and low resource consumption.

Upstream documentation: [https://www.ory.sh/docs/hydra/self-hosted/quickstart](https://www.ory.sh/docs/hydra/self-hosted/quickstart)

##### Kratos

Ory Kratos is a developer-friendly, security-hardened and tested identity, user management and authentication system for the cloud.

Upstream documentation: [https://www.ory.sh/docs/kratos/quickstart](https://www.ory.sh/docs/kratos/quickstart)

##### Login UI

Canonical Login UI is an orchestration application to manage and redirect users to the right component.

##### Admin UI

Canonical Admin UI is a management application that enables manual configuration of different applications, creation of OAuth2 clients and modification of identity schemas using UI.
