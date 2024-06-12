Supported Juju Versions
=======================

The following sections describe which version of the Juju CLI or controller is required for different scenarios.

Deploying JAAS
--------------

In order to deploy JAAS and all its components you must use a Juju controller with a minimum version of **3.x**.

Juju 3.x is required for the support of Juju secrets.

Using JAAS
----------

In order to interact with JAAS as a user, you must use a Juju CLI with a minimum version of **3.5.1**.

JAAS performs login using OIDC, a feature introduced to the Juju CLI in 3.5.1 and above.

Add controllers to JAAS
-----------------------

JAAS supports communicating with Juju controllers of various versions.

JAAS supports Juju controllers with a minimum version **3.4**.

Controllers with a lower version do not have the necessary configuration options to work with JAAS.

Going forward, JAAS is versioned with the same major version number as Juju. This means JAAS will always
support Juju controllers with the same major version as itself. E.g. JAAS v3 should be used with Juju 3.x controllers.

Additionally, JAAS will also support the last LTS release from Juju's previous major release E.g. JAAS v4 will also
support the final Juju 3.x minor version.

More information on Juju's roadmap and release information can be found `here <https://juju.is/docs/juju/roadmap>`__.
