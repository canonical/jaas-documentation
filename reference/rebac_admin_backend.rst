ReBAC Admin Backend
=======================

The ReBAC Admin API is a REST API that provides various endpoints to query or 
manipulate relationships in JAAS ReBAC authorisation model.

.. hint::
    For an explanation on Relationship-Based Access Control (ReBAC) check out :doc:`this <../explanation/authorisation>` explanation topic.

.. hint::
    As a reference on JAAS authorisation model, check out :doc:`this <../reference/authorisation_model>` reference topic.

OpenAPI specification
---------------------
The OpenAPI spec can be found at this ``https://<jimm-deployment>/rebac/v1/swagger.json``

Authentication
--------------
These endpoints are meant to be called from a web browser, therefore the authentication is handled via Cookies.

JAAS Implementation
-------------------

JAAS implements a subset of the operations described in the OpenAPI spec. 

 ====== ================== =========================================================================
 Status Entities           Notes
 ====== ================== =========================================================================
   ✅   ``entitlements``     
   ✅   ``capabilities`` 
   ✅   ``groups``     
   ✅   ``resources``     
   🟡   ``identities``     no support for creation, update and deletion.
   ❌   ``roles``          no support for roles.
   ❌   ``authentication`` no support for authentication providers.
 ====== ================== =========================================================================
