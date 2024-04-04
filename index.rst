:relatedlinks: [Diátaxis](https://diataxis.fr/)

.. _home:

JAAS Documentation
==================

**JAAS** provides a single location to manage your Juju infrastructure by using the 
Dashboard or using the same Juju CLI commands to create a high-level overview and 
the ability to drill-in to the details when you need it.

**JAAS** is composed of the following components:

- Candid - a macaroon-based authentication server, which can use a variety of identity providers to correctly identify users,
- JIMM - Juju Intelligent Model Manager, which acts as a single point of contact for multiple Juju controllers,
- Juju controllers -  each controlling models in specific clouds or cloud regions,
- Juju dashboard - providing a clear overview of your Juju real estate with the ability to drill down into details of your deploys.

**JAAS** is useful for customers that do not want to maintain their own controllers
in public clouds. Canonical's JAAS enables users to deploy their workloads
in public clouds without the extra complexity and costs associated with running their
own Juju controllers. JAAS is also useful for organisations 
running their own Juju infrastructure giving them a single point of contact for 
their entire real estate and, in combination with the Juju Dashboard, giving
them a clear overview of their infrastructure.

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

Project and community
---------------------

JAAS is a member of the Ubuntu family and warmly welcomes community contributions, suggestions, fixes and constructive feedback.

* `Code of conduct <https://ubuntu.com/community/ethos/code-of-conduct>`_
* Join the `Mattermost community char <https://chat.charmhub.io/charmhub/channels/jaas>`_
* Report a bug on `Launchpad <https://bugs.launchpad.net/jaas-issue-tracking>`_
* Contribute to the :doc:`documentation <contribute>`
* Visit the `Canonical's careers page <https://canonical.com/careers>`_


.. toctree::
   :hidden:
   :maxdepth: 2

   tutorial/index
   how-to/index
   reference/index
   explanation/index

