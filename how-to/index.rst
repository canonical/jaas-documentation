How-to guides
=============

These how-to guides cover key operations and processes in JAAS.

Candid Deployment and Configuration
-----------------------------------

`Candid`_ serves as authentication service for JIMM. So you need a working Candid
before deploying JIMM. Depending on the platform/requirements, some details of
the Candid deployment could be different.

.. toctree::
   :maxdepth: 1

   Deploy Candid <deploy_candid>
   Deploy Candid on K8S <deploy_candid_k8s>
   Candid LDAP integration <candid_ldap_integration>
   Candid Azure integration <candid_azure_integration>
   Candid ADFS integration <candid_adfs_integration>


JIMM Deployment
---------------

Here are instructions on how to deploy JIMM.

.. toctree::
   :maxdepth: 1

   Deploy JIMM <deploy_jimm>
   Deploy JIMM on K8S <deploy_jimm_k8s>


JIMM Configuration
------------------

After JIMM has been deployed, you need to configure it with your Juju-operated cluster.

.. toctree::
   :maxdepth: 1

   Add controller <add_controller>
   Add controller without DNS <add_controller_no_dns>
   Set up Route53 <route53>

Terraform
---------

.. toctree::
   :maxdepth: 1

   Using Terraform <use_terraform>