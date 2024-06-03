Bootstrap Admin Permissions
===========================

The following document will show you how to add permissions for an initial admin user to your JAAS environment.

Prerequisites
-------------

For this how-to you will need the following:

- A basic understanding of JAAS tags, see our :doc:`explanation doc<../explanation/jaas_tags>`.
- A running JAAS environment, see :doc:`our tutorial <../tutorial/deploy_jimm_microk8s>`.
- An understanding of Juju permissions, see the `Juju docs <https://juju.is/docs/juju/user-permissions>`__.

Creating an admin user
----------------------

In order to create an initial admin user we must use the config option ``controller-admins``.

The format for ``controller-admins`` is a space separated list of email addresses or service accounts. This means
that entries can be of the form ``name@domain.com`` or ``client-id@serviceaccount``.

Run the following command replacing the contents with your email address to configure your user as an JIMM admin.

.. code:: bash

    juju config jimm controller-admins="username@domain.com"

Now you can verify that you have admin access to JIMM using ``jimmctl``.

The following commands are particularly useful.

.. code:: bash

    jimmctl controllers
    jimmctl audit-events

In a fresh setup, the first should return an empty list, showing that no controllers have been added to JIMM.

The second command returns a list of audited events that JIMM has recorded. More information on JIMM's audit log feature
is available at the following :doc:`page<../reference/audit_logs>`.

Granting permissions
--------------------

As a JIMM admin, you are automatically an administrator of all controllers and models on those controllers.

Permissions to resources can now be handled in one of two ways.

1. Through ``juju``

All Juju permission related commands are valid with JIMM. This is the expected approach for all users to manage permissions 
to resources they own.

The following example will create a model and grant a fictional user read access to the model.

.. code:: bash

    juju add-model permission-test
    juju grant foo@canonical.com read permission-test

This allows other users to see your model provided they have logged into JIMM.

2. Using ``jimmctl``

Admins of JIMM can use ``jimmctl`` to view permissions on a more granular level and perform group management.

.. code:: bash

    # View all relations
    jimmctl auth relation list
    # Check if a user has access to a resource
    jimmctl auth relation check user-foo@canonical.com administrator controller-jimm
    # Add a group
    jimmctl auth group add my-group
    # Add user to a group
    jimmctl auth relation add user-foo@canonical.com member group-my-group
    # View members of a group
    jimmctl auth relation list --target group-my-group

More details on group management are available in our :doc:`group and access management tutorial<../tutorial/group_management>`.