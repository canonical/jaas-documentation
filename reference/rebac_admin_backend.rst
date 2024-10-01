ReBAC Admin Backend
=======================

The following sections briefly describe the functionalities offered by the ReBAC Admin Backend.

OpenAPI specification
---------------------
The OpenAPI spec can be found at this `repository <https://github.com/canonical/openfga-admin-openapi-spec>`__

Authentication
--------------
These endpoints are meant to be called from a web browser, therefore the authentication is handle via Cookies.

JAAS Implementation
-----------------

We implement a subset of the operations described in the OpenAPI spec. 

 ====== ============= =========================================================================
 Status Entities      Notes
 ====== ============= =========================================================================
   ✅   capabilities     
   ✅   entitlements     
   ✅   capabilities     
   ✅   groups     
   ✅   resources     
   🟡   identities    no support for creation, update and deletion.
   ❌   roles         no support for roles.
 ====== ============= =========================================================================



