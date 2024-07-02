JAAS: Authorisation Model
=========================

JAAS uses a Relationship-Based Access Control (ReBAC) scheme for authorisation purposes. This document illustrates the underlying authorisation model used by JAAS.

.. hint::
    For an explanation on Relationship-Based Access Control (ReBAC) check out :doc:`this <../explanation/jaas_authorisation>` explanation topic.

.. hint::
    As a reference on manipulating authorisation data, check out :doc:`this <./authorisation_data>` reference topic.


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


