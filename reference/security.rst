JAAS Security
=============

This document is an overview of JAAS specifically covering areas pertaining to security and/or sensitive data.

In each section we explain how the information is stored/transmitted and any cryptographic
technologies used.

Cloud-Credentials
-----------------

Cloud-credentials are the API keys Juju/JAAS needs in order to provision cloud infrastructure.
These keys are uploaded to and stored by JAAS. Whenever a a model is created, the key is uploaded to 
the Juju controller and used to manage cloud resources.

User provided cloud credentials are stored securely and safely within a secret `vault <https://www.vaultproject.io/>`__. 
It is  essential to handle these secrets securely to prevent unauthorised access and data breaches
to the user's cloud.

The use of Vault ensures that credentials are encrypted at rest and provides tools to avoid unauthorised
access.

JAAS - Juju Communication
--------------------------

JAAS acts as a central auth gateway between users and Juju controllers.
Juju controllers explicitly trust JAAS by setting the ``login-token-refresh-url`` at bootstrap
time. See our :doc:`how-to <../how-to/add_controller>` for more info on how to setup a new
Juju controller for JAAS.

Trust between Juju controllers and JAAS is established through the use of asymmetric cryptography
and `JSON Web Tokens <https://jwt.io/introduction>`__ (JWT). 

.. hint::

    To avoid confusion, note that JWTs are also used elsewhere in JAAS particularly in user sessions. 
    Although solving a similar problem, the tokens used between JAAS and Juju and those issued to users
    are two separate systems.

The ``login-token-refresh-url`` config option points to a
`JSON Web Key Set <https://auth0.com/docs/secure/tokens/json-web-tokens/json-web-key-sets>`__ 
(JWKS) endpoint. This endpoint contains a set of public keys used to verify the authenticy of tokens
issued by JAAS.

Whenever a request is made by JAAS to a Juju controller, a JWT is issued that encodes the user's
information and permissions. This mechanism allows the authorisation and authentication to be securely
delegated from the Juju controller to JAAS.

Specific details are below:

- **JWKS endpoint:** ``<jimm-url>/.well-known/jwks.json``
- **Key Type/Size:** RSA 4096 bits
- **Signing Algorithm:** RS256

The following Go packages are used to create the JWKS and JWTs.

- `<github.com.com/lestrrat-go/jwx/v2/jwa>`__
- `<github.com/lestrrat-go/jwx/v2/jwt>`__
- `<github.com/lestrrat-go/jwx/v2/jwk>`__

User Sessions
-------------

CLI Based Sessions
^^^^^^^^^^^^^^^^^^

When authenticating the Juju CLI to JAAS the user goes through an OAuth login flow (see `Device Code Flow`_).
After login, the CLI is issued a JWT i.e. a session token. This avoids making the user login repeatedly.

.. hint::
  These JWTs use the same technology as the tokens used in communications between JAAS and Juju controllers but
  they are not the same tokens.

The session token is eventually stored on the file-system and sent on each request.

Whereas the tokens issued to Juju controllers use asymmetric cryptography, tokens issued to users are signed with
symmetric cryptography. Session tokens are signed using a cryptographic hash function and a shared secret. 
Since there is no need for external parties to verify sessions, only JAAS, who holds the shared secret can verify
the session token. 

Note that the JWT contains the user's email and should be secured to avoid a a possible leak of personal information
(in addition to possible malicious use of an account).

Specific details are below:

- **Key Size:** >=512 bits
- **Signing Algorithm:** HS256

The following Go packages are used for creating JWTs:

- `<github.com/lestrrat-go/jwx/v2/jwt>`__
- `<github.com/lestrrat-go/jwx/v2/jwa>`__

Browser Cookies
^^^^^^^^^^^^^^^

When using JAAS from the Juju dashboard, a different type of session is created. In this scenario cookies are
used as browsers offer better built in support for cookies.

After login is completed, the browser is issued with a session cookie. The cookie's stores a cryptographically
encoded value that allows the server to retrieve the session data.

Specific details are below:

- **Key Size:** >=512 bits
- **Signing Algorithm:** HMAC-SHA256 (the same as HS256 but HS256 is normally used in the context of JWTs)

Note that the browser session cookie does not contain any information beyond an encoded session ID that is
used to lookup the user information. This is in contrast with the CLI session token which contains the user's email.

The following Go libraries are used to handle browser sessions:

- `<github.com/gorilla/sessions>`__
- `<github.com/antonlindstrom/pgstore>`__

OIDC Authentication
-------------------

JAAS employs OAuth 2.0 and OpenID Connect (OIDC) for user authentication, 
see `here <https://developer.okta.com/docs/concepts/oauth-openid/>`__
for an overview on these concepts.

Below we outline the various login flows used in JAAS.

The following Go packages are used to implement this functionality:

- `<golang.org/x/oauth2>`__
- `<golang.org/x/oauth2/clientcredentials>`__
- `<github.com/coreos/go-oidc/v3/oidc>`__

Authorisation Code Flow
^^^^^^^^^^^^^^^^^^^^^^^

When performing login via a browser, users go through the authorisation code flow.
This is best described by various diagrams such as 
`this <https://auth0.com/docs/get-started/authentication-and-authorization-flow/authorization-code-flow>`__.

In this flow the user's browser is redirected to the identity provider where they are
asked to login before being redirected back to the original application.

Because JAAS is a server side app, the access token and refresh token are stored
by the back-end application and a session cookie is issued to the browser as described in 
`Browser Cookies`_.

To protect against `CSRF attacks <https://auth0.com/docs/secure/attack-protection/state-parameters>`__
the back-end application issues a random nonce used in the ``state`` parameter of the OAuth
flow. This prevents a malicious attacker from forging a request to login as another user.

Device Code Flow
^^^^^^^^^^^^^^^^

When performing login via the device flow, the Juju CLI will:

1. Request from JAAS a URL and random code which the user can use to login.
2. The CLI will then wait for a response from JAAS.
3. JAAS polls the identity server, which in turn is waiting for the user to complete their login.

A diagram depicting this login flow can be found 
`here <https://auth0.com/docs/get-started/authentication-and-authorization-flow/authorization-code-flow>`__.

This process does not rely on browser redirects and so is not susceptible to traditional browser vulnerabilities.
Both the access token and refresh token are obtained and stored by the backend server
and the CLI application is issued with a session token as described in `CLI Based Sessions`_.

Client Credential Flow
^^^^^^^^^^^^^^^^^^^^^^

When there is a need to perform login by a machine rather than a physical user, OAuth handles this through
the use of a client credential flow.

A diagram depicting this login flow can be found 
`here <https://auth0.com/docs/get-started/authentication-and-authorization-flow/client-credentials-flow>`__.

This process is reserved for scenarios where machine to machine authentication is required. A good example
of this is includes the use of the Juju Terraform Provider where the client-credential flow is employed.

In the link above, the client application communicates with the identity provider to retrieve an access token
that is then used with the API server. This design is shifted in JAAS. The client application instead sends
its application credentials to JAAS which then forwards the information to the identity provider. JAAS
effectively acts as a proxy between the client application and the identity provider.

This scheme simplifies authentication for client applications but is only possible since JAAS is a trusted
application in the system.

Macaroons & Offer Authentication
--------------------------------

Macaroons are a tool for decentralised authentication similar to JSON Web Tokens.
The `Go Macaroon package <https://pkg.go.dev/gopkg.in/macaroon.v2@v2.1.0>`__ is used by JAAS and has more
details on the low-level operations that Macaroons are capable of.

Macaroons are used by Juju for various purposes but in JAAS their primary purpose is for authorising 
cross-model relations between controllers. When two Juju controllers (that are connected to JAAS) 
communicate for the purposes of sharing an application offer, JAAS acts as the source of truth for 
authorisation data. These checks are handled using macaroons.

Macaroons use a combination of HMAC for cryptographic signatures and symmetric encryption to encode
the scope (or caveats) of what a macaroon is entitled to.

These operations are performed using ``HMAC-SHA256`` and ``XSalsa20-Poly1305``. The following Go 
packages are used by the underlying macaroon package for these operations:

- ``crypto/hmac``
- ``crypto/sha256``
- `<golang.org/x/crypto/nacl/secretbox>`__

Additionally, the higher-level `Macaroon Bakery package <https://github.com/go-macaroon-bakery/macaroon-bakery>`__
is used to interface with macaroons and introduces public key cryptography to perform similar operations
as mentioned above. This allows services to trust macaroons generated externally.

These operations are performed using ``Ed25519`` and ``XSalsa20-Poly1305``. The following Go packages are 
used by the underlying macaroon bakery package for these operations:

- `<golang.org/x/crypto/nacl/box>`__
- `<golang.org/x/crypto/curve25519>`__

When a Juju controller is connected to JAAS, the ``login-token-refresh-url`` is used to determine where 
the JAAS macaroon public key is located. This public key is used when Juju controllers issue macaroons 
and enforces that the macaroon can only be  discharged by JAAS, who holds the private key. Discharging 
a macaroon refers to the process of verifying its claims.

Specific details are below:

- **Macaroon Public Key endpoint:** ``<jimm-url>/macaroons/publickey``
- **Key Type:** Ed25519 (256-bit key)
- **Signing algorithm:** Ed25519

TLS Communication
-----------------

In this section we will cover the use of TLS between components of JAAS.

TLS encryption is handled by the Go standard library packages:

- ``crypto/tls``
- ``crypto/x509``

Client - JAAS
^^^^^^^^^^^^^

The Juju client package enforces the use of TLS when connecting to a controller.
This extends to both the Juju CLI and the Juju Terraform Provider.

The minimum supported version is TLS v1.2

JAAS - Juju Controllers
^^^^^^^^^^^^^^^^^^^^^^^

JAAS enforces the use of TLS when connecting to a Juju controller.

The minimum supported version is TLS v1.2

JAAS - OpenFGA
^^^^^^^^^^^^^^

JAAS does not currently enforce TLS when communicating with OpenFGA.

TLS is not currently supported with the OpenFGA charm operator.

JAAS - Vault
^^^^^^^^^^^^

JAAS assumes that Vault is reachable with TLS but does not enforce this.
By default the Vault charm employs the use of TLS.

The minimum supported version is TLS v1.2.

JAAS - PostgreSQL
^^^^^^^^^^^^^^^^^

JAAS does not currently enforce TLS when communicating with PostgreSQL.
But this can be achieved when using the PostgreSQL charm.

The minimum supported version is TLS v1.2.

CORS
----

CORS or Cross-Origin Resource Sharing is a browser security feature designed to prevent
malicious use of your online credentials. Read more on CORS 
`here <https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#what_requests_use_cors>`__.

JAAS supports the use of CORS headers specifically for the purposes of the Juju
dashboard. The dashboard requires the ability to send cross-origin requests from the domain
where it is hosted to the domain where JAAS is hosted. More information on how to setup
CORS to securely handle these requests will be available in a future how-to.

The following Go package is used to validate CORS requests/headers:

- `<github.com/rs/cors>`__

