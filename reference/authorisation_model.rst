JAAS: Authorisation Model
=========================

JAAS uses Relationship-Based Access Control (ReBAC) scheme for authorisation purposes. This document illustrates the underlying authorisation model used by JAAS.

.. hint::
    For an explanation on Relationship-Based Access Control (ReBAC) check out :doc:`this <../explanation/jaas_authorisation>` explanation topic.

The model
---------

JAAS authorisation model reshapes the `Juju permission model <https://juju.is/docs/juju/user-permissions>`_ into a ReBAC paradigm. The OpenFGA authorisation model used by JAAS is defined as:

.. code:: text

    model
        schema 1.1

    type applicationoffer
    relations
        define administrator: [user, user:*, group#member] or administrator from model
        define consumer: [user, user:*, group#member] or administrator
        define model: [model]
        define reader: [user, user:*, group#member] or consumer


    type cloud
    relations
        define administrator: [user, user:*, group#member] or administrator from controller
        define can_addmodel: [user, user:*, group#member] or administrator
        define controller: [controller]

    type controller
    relations
        define administrator: [user, user:*, group#member] or administrator from controller
        define audit_log_viewer: [user, user:*, group#member] or administrator
        define controller: [controller]

    type group
    relations
        define member: [user, user:*, group#member]

    type model
    relations
        define administrator: [user, user:*, group#member] or administrator from controller
        define controller: [controller]
        define reader: [user, user:*, group#member] or writer
        define writer: [user, user:*, group#member] or administrator

    type serviceaccount
    relations
        define administrator: [user, user:*, group#member]

    type user

Here is the directed graph illustration of the above model. In this figure, purple and green nodes represent entity types and relations, respectively. The dashed lines show the internal indirect relationships among relations defined on the entity type.

.. image:: images/authorisation_model.png


Tuples
------

In ReBAC, a *tuple* is a plain data structure that represents the relationship between two entities. So, a tuple has three components:

1. Entity A (or *Object*): the entity that receives (or uses) the relation.
2. Relation: the type of relationship.
3. Entity B (or *Target*): the entity that provides the relation.

.. note::
    JAAS terminology is slightly different from OpenFGA. In OpenFGA, *Entity A* and *Entity B* are called *User* and *Object*, but in JAAS, they are called *Object* and *Target (object)*. The reason behind this is to prevent confusion over real users and relationship users.

For example, if a ``user`` named ``alice@canonical.com`` has the ``member`` relationship with a group named ``foo``, then the tuple that represents this relation will look like this:

.. code:: yaml

    object:   user:alice@canonical.com
    relation: member
    target:   group:foo

This reads as: "an entity of type ``user``, named ``alice@canonical.com``, has ``member`` relationship to an entity of type ``group``, named ``foo``.


Manipulating tuples
-------------------

.. hint::
    For a tutorial on managing users/groups permissions, check out :doc:`this <../tutorial/group_management>` topic.

To manipulate the tuples (i.e. add/remove relations between different resources), you can use the ``jimmctl auth`` commands. For example, the command below adds the tuple discussed in the last example:

.. code:: bash

    jimmctl auth relation add user-alice@canonical.com member group-foo


To check if a specific tuple exists, you use the ``relation check`` command:

.. code:: bash

    jimmctl auth relation check user-alice@canonical.com member group-foo


You can also remove a tuple with a similar syntax:

.. code:: bash

    jimmctl auth relation remove user-alice@canonical.com member group-foo


