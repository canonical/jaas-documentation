JAAS: Security Scope
========================
The scope of JAAS' security covers multiple aspects, including:

Secure Communication
~~~~~~~~~~~~~~~~~~~~
JAAS ensures secure communication between controllers (management nodes) and models (namespaces)
by means of authorisation, authentication and TLS encryption to prevent unauthorised access
and prevent data interception or tampering.

Additionally, when communicating with any controller, JAAS uses token based authorisation whilst
additionally acting as the IdP (Identity Provider) for said tokens.

Access control and authentication
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
JAAS provides an abstraction layer of access control atop of Juju. JAAS does this by backing its users
with an IdP (Identity Provider), this particular IdP is called Candid which handles the authorisation 
and authentication for JIMM (Juju Intelligent Model Manager, the backend to JAAS). Furthermore, JAAS is 
capable of many authentication and authorisation types, ranging from users to agents.

Auditing and logging
~~~~~~~~~~~~~~~~~~~~
JAAS provides audit logs of all access to each model managed by JAAS and by which user.
Currently, JAAS does not provide a way to view the logs of applications within models.

Model and user isolation
~~~~~~~~~~~~~~~~~~~~~~~~
JAAS, like Juju, enables users with the correct cloud credentials associated with their user
to create models [under that clouds controller] for different applications, services, and 
environments via the JAAS controller. 

Secure storage and handling of secrets
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
User provided cloud credentials are stored securely and safely within a secret vault. It is 
essential to handle these secrets securely to prevent unauthorised access and data breaches
to the users cloud.
