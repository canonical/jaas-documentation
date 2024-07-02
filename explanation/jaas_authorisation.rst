JAAS Authorisation
==================

JAAS provides enterprise-level features on top of Juju. One of such features is the `Relationship-Based Access Control (ReBAC) <https://en.wikipedia.org/wiki/Relationship-based_access_control>`_ that enables enterprises to have more control over users and their permissions to access underlying Juju resources (e.g. controllers or models). Among various implementations, JAAS uses `OpenFGA <https://openfga.dev/>`_ as the backend for ReBAC. You can read more about Juju permission model in `here <https://juju.is/docs/juju/user-permissions>`_.

.. hint::
    For a tutorial on managing users/groups permissions, check out :doc:`this <../tutorial/group_management>` topic.

.. hint::
    More details about the JAAS authorisation model is available in :doc:`references <../reference/authorisation_model>`.


What is ReBAC?
--------------

Unlike `Role-Based Access Control (RBAC) <https://en.wikipedia.org/wiki/Role-based_access_control>`_ where permission sets are managed by the concept of *roles*, in ReBAC, a user's access to a resource is modeled through a *relation*, which could be either direct or indirect (i.e. the result of another relation). This makes ReBAC more dynamic in comparison to RBAC, and also more suitable for complex authorisation schemes where there are large number of users and resources.

As an example, consider a simple file-system structure with two kinds of resources: directories and files. Without ReBAC, you need to be explicit about every user's permissions (or set of permissions, as roles) to every file or directory. But, with ReBAC, you can achieve the same result with much less effort and data, by defining the right relations. For instance, you can just assign the ``read::directory:foo`` relation to a user (meaning that the user has ``read`` relation to the ``directory`` named ``foo``), and then the user will have the read access to all files and directories under ``foo``. Note that, you only declared *one* relationship, and the other relations are automatically inferred from that.
