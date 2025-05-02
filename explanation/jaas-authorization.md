(jaas-authorization)=
# Authorization

JAAS provides enterprise-level features on top of Juju. One such feature is enhanced authorization, which provides enterprises with more control over user permissions to access underlying Juju resources (e.g., controllers or models).

JAAS reshapes Juju's permission model based on access levels into the more flexible [Relationship-Based Access Control (ReBAC)](https://en.wikipedia.org/wiki/Relationship-based_access_control) paradigm and also expands the list of entities that can be involved in access: With Juju access levels a user is granted access to another entity. With JAAS relations, a user, service account, role or group is brought into a relation with another entity, where the relation is about access to the entity.

At present JAAS relations are parallel to {external+juju:ref}`Juju access levels <user-access-levels>`, but in the future they're expected to become a superset thereof.

## What is ReBAC?

Unlike [Role-Based Access Control (RBAC)](https://en.wikipedia.org/wiki/Role-based_access_control) where permission sets are managed by the concept of *roles*, in ReBAC, a user's access to a resource is modelled through a *relationship*, which can be either direct or indirect (the result of another relationship). This makes ReBAC more dynamic in comparison to RBAC, and also more suitable for complex authorization schemes where there are large number of users and resources.

As an example, consider a simple file-system structure with two kinds of resources: directories and files. Without ReBAC, you need to be explicit about every user's permissions (or set of permissions, as roles) to every file or directory. But, with ReBAC, you can achieve the same result with much less effort and data, by defining the right relations. For instance, you can assign the `read::directory:foo` relation to a user (meaning that the user has `read` relation to the `directory` named `foo`), and then the user will have the read access to all files and directories under `foo`. Note that, you only declared *one* relationship (or more precise, *tuple*), and the other relations are automatically inferred from that.


## JAAS authorization components

Conceptually, the JAAS authorization system consists of two main components:

1. **Authorization model**, which defines the schema of different entity types (e.g. controllers, users, or groups), the possible relationships between them (e.g. group memberships, or administrator relation for controllers), and the inheritance structure for permissions (e.g. a controller administrator is also an administrator for all models on that controller).

````{dropdown} View the authorization model (diagram)

Note: Directed graph illustration of the JAAS authorization model. Purple and green nodes represent entity types and relations, respectively. The dashed lines show the internal indirect relationships among relations defined on the entity type (e.g., an entity can have the `reader`, `writer`, or `administrator` relation to a `model`). Note: The `controller` and `model` relations are implicit internal relations that describe the inheritance structure for permissions (e.g., the fact that a cloud/model is always associated with a controller or an offer with a model, and permissions on the latter carry over to the former).

```{figure} jaas-authorization-model.png
   :width: 600px
   :alt: JAAS authorization model
```
````

````{dropdown} View the authorization model (source)

Note: The `controller` and `model` relations are implicit internal relations that describe the inheritance structure for permissions (e.g., the fact that a cloud/model is always associated with a controller or an offer with a model, and permissions on the latter carry over to the former).

```text
# copy me into https://play.fga.dev to update png

model
  schema 1.1

type user

type role
  relations
    define assignee: [user, user:*, group#member]

type group
  relations
    define member: [user, user:*, group#member]

type controller
  relations
    define administrator: [user, user:*, group#member, role#assignee] or administrator from controller
    define audit_log_viewer: [user, user:*, group#member, role#assignee] or administrator
    define controller: [controller]

type model
  relations
    define administrator: [user, user:*, group#member, role#assignee] or administrator from controller
    define controller: [controller]
    define reader: [user, user:*, group#member, role#assignee] or writer
    define writer: [user, user:*, group#member, role#assignee] or administrator

type applicationoffer
  relations
    define administrator: [user, user:*, group#member, role#assignee] or administrator from model
    define consumer: [user, user:*, group#member, role#assignee] or administrator
    define model: [model]
    define reader: [user, user:*, group#member, role#assignee] or consumer

type cloud
  relations
    define administrator: [user, user:*, group#member, role#assignee] or administrator from controller
    define can_addmodel: [user, user:*, group#member, role#assignee] or administrator
    define controller: [controller]

type serviceaccount
  relations
    define administrator: [user, user:*, group#member, role#assignee]

```
````


2. **Tuples (or relationship data)**, which represent a set of individual relationships between concrete entities (e.g., a user named *foo* is an *admin* of a controller named *bar*).

Inherently, the authorization model is a static component and cannot be changed by the administrators of JAAS. On the other hand, the tuples, are dynamic data and JAAS provides tools for administrators to manipulate them.


````{dropdown} View all the tuple templates arising from the authorization model

Note: The `controller` and `model` relations are implicit internal relations that describe the inheritance structure for permissions (e.g., the fact that a cloud/model is always associated with a controller or an offer with a model, and permissions on the latter carry over to the former).

```text

(applicationoffer:some_offer, model, model:some_model)
(applicationoffer:some_offer, administrator, user:some_user)
(applicationoffer:some_offer, administrator, user:*)
(applicationoffer:some_offer, administrator, group:some_group#member)
(applicationoffer:some_offer, administrator, role:some_role#assignee)
(applicationoffer:some_offer, administrator, model:some_model#administrator)
(applicationoffer:some_offer, consumer, user:some_user)
(applicationoffer:some_offer, consumer, user:*)
(applicationoffer:some_offer, consumer, group:some_group#member)
(applicationoffer:some_offer, consumer, role:some_role#assignee)
(applicationoffer:some_offer, consumer, applicationoffer:some_offer#administrator)
(applicationoffer:some_offer, reader, user:some_user)
(applicationoffer:some_offer, reader, user:*)
(applicationoffer:some_offer, reader, group:some_group#member)
(applicationoffer:some_offer, reader, role:some_role#assignee)
(applicationoffer:some_offer, reader, applicationoffer:some_offer#consumer)

(cloud:some_cloud, controller, controller:some_controller)
(cloud:some_cloud, administrator, user:some_user)
(cloud:some_cloud, administrator, user:*)
(cloud:some_cloud, administrator, group:some_group#member)
(cloud:some_cloud, administrator, role:some_role#assignee)
(cloud:some_cloud, administrator, controller:some_controller#administrator)
(cloud:some_cloud, can_addmodel, user:some_user)
(cloud:some_cloud, can_addmodel, user:*)
(cloud:some_cloud, can_addmodel, group:some_group#member)
(cloud:some_cloud, can_addmodel, role:some_role#assignee)
(cloud:some_cloud, can_addmodel, cloud:some_cloud#administrator)

(controller:some_controller, controller, controller:some_other_controller)
(controller:some_controller, administrator, user:some_user)
(controller:some_controller, administrator, user:*)
(controller:some_controller, administrator, group:some_group#member)
(controller:some_controller, administrator, role:some_role#assignee)
(controller:some_controller, administrator, controller:some_controller#administrator)
(controller:some_controller, audit_log_viewer, user:some_user)
(controller:some_controller, audit_log_viewer, user:*)
(controller:some_controller, audit_log_viewer, group:some_group#member)
(controller:some_controller, audit_log_viewer, role:some_role#assignee)
(controller:some_controller, audit_log_viewer, controller:some_controller#administrator)

(group:some_group, member, user:some_user)
(group:some_group, member, user:*)
(group:some_group, member, group:some_other_group#member)

(model:some_model, controller, controller:some_controller)
(model:some_model, administrator, user:some_user)
(model:some_model, administrator, user:*)
(model:some_model, administrator, group:some_group#member)
(model:some_model, administrator, role:some_role#assignee)
(model:some_model, administrator, controller:some_controller#administrator)
(model:some_model, reader, user:some_user)
(model:some_model, reader, user:*)
(model:some_model, reader, group:some_group#member)
(model:some_model, reader, role:some_role#assignee)
(model:some_model, reader, model:some_model#writer)
(model:some_model, writer, user:some_user)
(model:some_model, writer, user:*)
(model:some_model, writer, group:some_group#member)
(model:some_model, writer, role:some_role#assignee)
(model:some_model, writer, model:some_model#administrator)

(role:some_role, assignee, user:some_user)
(role:some_role, assignee, user:*)
(role:some_role, assignee, group:some_group#member)

(serviceaccount:some_account, administrator, user:some_user)
(serviceaccount:some_account, administrator, user:*)
(serviceaccount:some_account, administrator, group:some_group#member)
(serviceaccount:some_account, administrator, role:some_role#assignee)

```
````