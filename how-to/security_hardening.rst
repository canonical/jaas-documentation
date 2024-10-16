JAAS: Security Hardening
=======================
JIMM, the service at the centre of JAAS can be hardened in a number of ways. This 
document details how you can harden the security of your JAAS deployment. 

CORS
----
To set CORS on JIMM, use the configuration option "cors-allowed-origins".

LEGO & NGINX Ingress Integrator
------------------------
The NGINX ingress integator is a a charm responsible for creating Kubernetes ingress rules, 
these rules can be hardened via TLS and the charm provides a means to do so. See `here <https://charmhub.io/nginx-ingress-integrator>`__.

Our LEGO charms provide certificates for charms from a desired ACME server and can be integrated
with the integrator to enable TLS at the ingress level. See `here <https://charmhub.io/httprequest-lego-k8s>`__.

You will require a domain that your ACME is aware of and an nginx ingress controller installed
on your Kubernetes cluster.

With JAAS deployed, you can deploy both LEGO and the Inregator, and integrate your LEGO charm deployment
to your ingress integrator, and then the ingress integrator to JIMM to enable TLS ingress for

Identity Provider
-----------------
JAAS uses the Canonical Identity Platform for authentication. The communication between JAAS
and the identity platform can be secured via TLS.

You will require the identity Platform and the self-signed-certificates charm deployed.
See `here <https://charmhub.io/topics/canonical-identity-platform/tutorials/e2e-tutorial>`__ for deploying the identity platform. 

Your identity platform will require TLS enabled via the `self signed certificates charm <https://charmhub.io/self-signed-certificates>`__.

Using JIMM's receive-ca-cer integration, you can now relate to the self-signed-certificates charm
to enabled TLS between the identity platform and JIMM.

OpenFGA
-------
JIMM uses OpenFGA for authorisation and currently, the OpenFGA charm does not support TLS.

Vault
-----
TLS is enabled by default when communicating with the Vault charm.

JIMM uses Vault for storing cloud credentials, JWKS, and other secrets.

Juju Controllers
----------------
TLS is enabled by default when communicating with controllers.

When adding a Juju controller to JIMM, the self signed certificate of the controller is given to
JIMM.


PostgreSQL
----------
JIMM uses PostgreSQL as its persistent storage layer. The communication between PostgeSQL can be encrypted
via TLS. To enable TLS for charmed Postgresql you can follow this `guide <https://charmhub.io/postgresql-k8s/docs/t-enable-tls?channel=14/stable>`__.

    As of October 2024, you need to manually restart JIMM if you enable TLS on Postgres after having related the JIMM and PostgreSQL charms.
