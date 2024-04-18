JAAS Overview
=============

What is JAAS?
-------------

**JAAS** provides a single location to manage your Juju infrastructure by using the 
Dashboard or using the same Juju CLI commands to create a high-level overview and 
the ability to drill-in to the details when you need it.


**JAAS** is useful for customers that do not want to maintain their own controllers
in public clouds. Canonical's JAAS enables users to deploy their workloads
in public clouds without the extra complexity and costs associated with running their
own Juju controllers. JAAS is also useful for organisations 
running their own Juju infrastructure giving them a single point of contact for 
their entire real estate and, in combination with the Juju Dashboard, giving
them a clear overview of their infrastructure.

Architecture
------------

The diagram below shows an overall picture of JAAS architecture.

.. image:: images/jaas.png

As in the diagram JAAS consists of two main components: *Juju Intelligent Model Manager (JIMM)*
and *ReBAC* Authorization. Basically, JIMM implements a number of Juju facades and behaves as a
*Juju Controller*, which under the hood proxies operations to underlying controllers. This enables
other tools like Juju Dashboard or Juju CLI that expect a Juju Controller to communicate with, to
seamlessly work with JIMM.

For authentication of users or service accounts, JAAS requires an *OIDC Provider* that handles
the standard OAuth flows including browser flow, device flow, and client credentials.
