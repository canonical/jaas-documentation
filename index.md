(home)=
# JAAS Documentation

```{toctree}
:maxdepth: 2
:hidden: true

tutorial/index
how-to/index
reference/index
explanation/index
```


JAAS is an enterprise layer on top of [Juju](https://canonical-juju.readthedocs-hosted.com/en/latest/).

JAAS provides:

- The Juju Infinite Model Manager, JIMM (and its [backing charm](https://charmhub.io/juju-jimm-k8s)):  A Juju enterprise-level controller.

- JIMM-specific extensions to existing Juju machinery, including

  * the [Juju CLI](https://canonical-juju.readthedocs-hosted.com/en/latest/user/reference/juju-cli/) (enhanced with `jimmctl`, for JIMM admins, and `jaas`, for regular JIMM users),

  * the [Juju dashboard](https://canonical-juju.readthedocs-hosted.com/en/latest/user/reference/juju-dashboard/) (and its [backing charm](https://charmhub.io/juju-dashboard), and

  * the [Terraform Provider for Juju](https://canonical-terraform-provider-juju.readthedocs-hosted.com/en/latest/).

When you use an existing Juju on Kubernetes controller to deploy JIMM and its dependencies, and then connect your Juju controllers to JIMM, you gain the ability to:

- use OIDC authentication for integration with your existing identity provider for federated login, service accounts and other features offered by identity providers;
- use ReBAC for authorisation;
- use the Juju CLI, Juju Dashboard, and the Terraform Provider for Juju to interact with multiple Juju controllers from a single point of contact.

If you want to take Juju to the enterprise level, you need JAAS.

---------

## In this documentation

````{grid} 1 1 2 2

```{grid-item-card} [Tutorial](tutorial)
:link: tutorial/index
:link-type: doc

**Start here**: a hands-on introduction to Juju for new users
```

```{grid-item-card} [How-to guides](/index)
:link: how-to/index
:link-type: doc

**Step-by-step guides** covering key operations and common tasks
```

````

````{grid} 1 1 2 2
:reverse:

```{grid-item-card} [Reference](/index)
:link: reference/index
:link-type: doc

**Technical information** - specifications, APIs, architecture
```

```{grid-item-card} [Explanation](/index)
:link: explanation/index
:link-type: doc

**Discussion and clarification** of key topics
```

````

---------

## Project and community


JAAS is a member of the Ubuntu family and warmly welcomes community contributions, suggestions, fixes and constructive feedback.

* Read our [Code of Conduct ](https://ubuntu.com/community/code-of-conduct)
* Join our [Matrix chat](https://matrix.to/#/#jimm:ubuntu.com)
* Join our [Discourse forum](https://discourse.charmhub.io/)
* Report a bug in the [docs](https://github.com/canonical/jaas-documentation/issues) or the [code](https://github.com/canonical/jimm/issues)
* Contribute to the [docs](https://github.com/canonical/jaas-documentation/) or the [code](https://github.com/canonical/jimm)
* Visit the [Juju careers page](https://juju.is/careers)

