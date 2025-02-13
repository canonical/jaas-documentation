:relatedlinks: [Diátaxis](https://diataxis.fr/)

.. _home:

JAAS Documentation
==================

JAAS is an enterprise layer on top of `Juju <https://canonical-juju.readthedocs-hosted.com/en/latest/>`__.

JAAS provides:

- The Juju Infinite Model Manager, JIMM (and its `backing charm <https://charmhub.io/juju-jimm-k8s>`__):  A Juju enterprise-level controller.

- JIMM-specific extensions to existing Juju machinery, including

  * the `Juju CLI <https://canonical-juju.readthedocs-hosted.com/en/latest/user/reference/juju-cli/>`__ (enhanced with `jimmctl`, for JIMM admins, and `jaas`, for regular JIMM users),

  * the `Juju dashboard <https://canonical-juju.readthedocs-hosted.com/en/latest/user/reference/juju-dashboard/>`__ (and its `backing charm <https://charmhub.io/juju-dashboard>`__), and

  * the `Terraform Provider for Juju <https://canonical-terraform-provider-juju.readthedocs-hosted.com/en/latest/>`__.

When you use an existing Juju on Kubernetes controller to deploy JIMM and its dependencies, and then connect your Juju controllers to JIMM, you gain the ability to:

- use OIDC authentication for integration with your existing identity provider for federated login, service accounts and other features offered by identity providers;
- use ReBAC for authorisation;
- use the Juju CLI, Juju Dashboard, and the Terraform Provider for Juju to interact with multiple Juju controllers from a single point of contact.

If you want to take Juju to the enterprise level, you need JAAS.

---------

In this documentation
---------------------

..  grid:: 1 1 2 2

   ..  grid-item:: :doc:`Tutorial <tutorial/index>`


   ..  grid-item:: :doc:`How-to guides <how-to/index>`


.. grid:: 1 1 2 2
   :reverse:

   .. grid-item:: :doc:`Reference <reference/index>`


   .. grid-item:: :doc:`Explanation <explanation/index>`


---------

Where to begin
--------------

We recommend starting with our :doc:`JAAS overview<./explanation/jaas_overview>` and then our tutorial on :doc:`deploying JAAS to MicroK8s<./tutorial/deploy_jaas_microk8s>`.

This will help you understand the architecture of JAAS then guide you through its deployment with links to further docs on how to manage your environment.

Project and community
---------------------

JAAS is a member of the Ubuntu family and warmly welcomes community contributions, suggestions, fixes and constructive feedback.

* `Code of conduct <https://ubuntu.com/community/ethos/code-of-conduct>`_
* Join the `community chat on Matrix <https://matrix.to/#/#jimm:ubuntu.com>`_
* Report a bug on `Launchpad <https://bugs.launchpad.net/jaas-issue-tracking>`_
* Contribute to the :doc:`documentation <contribute>`
* Visit `Canonical's careers page <https://canonical.com/careers>`_


.. toctree::
   :hidden:
   :maxdepth: 2

   tutorial/index
   how-to/index
   reference/index
   explanation/index

