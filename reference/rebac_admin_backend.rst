ReBAC Admin Backend
=======================

The following sections briefly describe the functionalities offered by the ReBAC Admin Backend.

OpenAPI specification
---------------------
The OpenAPI spec can be found at this endpoint for your deployment: `https://<jimm-deployment>/v1/rebac/swagger.json`

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



