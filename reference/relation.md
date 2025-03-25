(relation)=
# Relation
> See first: {external+juju:ref}`Juju | User access levels <user-access-levels>`
>
> See also: {ref}`manage-relations`


```{dropdown} JAAS relations vs. Juju relations
JAAS relations have nothing to do with [Juju relations](https://canonical-juju.readthedocs-hosted.com/en/latest/user/reference/relation/), which refer to the relationship, or rather integration, of two charmed applications. However, JAAS relations are related to [Juju user access levels](https://canonical-juju.readthedocs-hosted.com/en/latest/user/reference/user/).
```

```{dropdown} JAAS relations vs. Juju access levels
JAAS reshapes Juju's permission model based on access levels into the more flexible [ReBAC](https://openfga.dev/docs/authorization-concepts#what-is-relationship-based-access-control) paradigm and also expands the list of entities that can be involved in access: With Juju access levels a user is granted access to another entity. With JAAS relations, a user, service account, role or group is brought into a relation with another entity, where the relation is about access to the entity.

> Note: JAAS relations are currently parallel to Juju access levels, but in the future they're expected to become a superset thereof.

```

<!--following [Relation Based Access Control (**ReBAC**)](https://en.wikipedia.org/wiki/Relationship-based_access_control) and using [OpenFGA](https://openfga.dev/docs/concepts#what-is-a-relation),-->

In JAAS, a relation is a string in an [OpenFGA ReBAC authorization model](https://openfga.dev/docs/authorization-concepts#what-is-relationship-based-access-control) that is part of a tuple consisting of an entity type A (in OpenFGA: 'user'; in JAAS: 'object'), the relation, and an entity type B (in OpenFGA: 'object'; in JAAS: 'target), where the relation is defined by type B and represents an entitlement of entity type A on the entity type B (i.e., it is about permission for A to perform an action on B).

For example:

```yaml
object:   user:alice@canonical.com
relation: member
target:   group:foo
```
reads as  "an entity of type `user`, named `alice@canonical.com`, has the `member` relation to an entity of type `group`, named `foo`".


````{dropdown} View the authorization model (diagram)

Note: Directed graph illustration of the JAAS authorization model. Purple and green nodes represent entity types and relations, respectively. The dashed lines show the internal indirect relationships among relations defined on the entity type (e.g., an entity can have the `reader`, `writer`, or `administrator` relation to a `model`). Note: The `controller` and `model` relations are implicit internal relations that describe the inheritance structure for permissions (e.g., the fact that a cloud/model is always associated with a controller or an offer with a model, and permissions on the latter carry over to the former).

```{figure} relation-authorization-model.png
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

```{dropdown} View the relations in their target entity context

Note: The `controller` and `model` relations are implicit internal relations that describe the inheritance structure for permissions (e.g., the fact that a cloud/model is always associated with a controller or an offer with a model, and permissions on the latter carry over to the former). As this is not something that a JAAS user can interact with, the sections link below omit them.

> See : {ref}`controller-relation`, {ref}`cloud-relation`, {ref}`model-relation`, {ref}`offer-relation`, {ref}`service-account-relation`, {ref}`role-relation`, {ref}`group-relation`
```


<!--
**JAAS entity names vs. OpenFGA ReBAC entity names.**

JAAS terminology is slightly different from OpenFGA. In OpenFGA, entity A and entity B are called 'user' and 'object', but in JAAS, they are called 'object' and 'target'.
-->


