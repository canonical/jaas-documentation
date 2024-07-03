JAAS: Authorisation Data
========================

JAAS uses a Relationship-Based Access Control (ReBAC) scheme for authorisation purposes. This document provides the information about the authorisation data structure.

.. hint::
    For an explanation on Relationship-Based Access Control (ReBAC) check out :doc:`this <../explanation/authorisation>` explanation topic.

.. hint::
    As a reference on JAAS authorisation model, check out :doc:`this <./authorisation_model>` reference topic.


Tuples
------

Simply, JAAS authorisation data is the set of relationships established between different entities. In ReBAC, a *tuple* is a plain data structure that represents the relationship between two entities. So, a tuple has three components:

1. Entity A (or *Object*): the entity that receives (or uses) the relation.
2. Relation: the type of relationship.
3. Entity B (or *Target*): the entity that provides the relation.

.. note::
    JAAS terminology is slightly different from OpenFGA. In OpenFGA, *Entity A* and *Entity B* are called *User* and *Object*, but in JAAS, they are called *Object* and *Target (object)*.

For example, if a ``user`` named ``alice@canonical.com`` has the ``member`` relationship with a group named ``foo``, then the tuple that represents this relation will look like this:

.. code:: yaml

    object:   user:alice@canonical.com
    relation: member
    target:   group:foo

This reads as: "an entity of type ``user``, named ``alice@canonical.com``, has the ``member`` relationship to an entity of type ``group``, named ``foo``.


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

