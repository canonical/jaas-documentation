(manage-permissions)=
# Manage permissions
> See first: {external+juju:ref}`Juju | Juju access levels <user-access-levels>`
>
> See also: {ref}`jaas-authorization`

(add-a-permission)=
## Add a permission

To add a permission between an entity A (always a user, whether identified directly or through a group/role) and an entity B (group, role, or resource -- controller, cloud, model, or application offer), run the `add-permission` command followed by A (in tag notation or alternatives), the desired B-supported permission, and B (in tag notation). For example:

```text
# Make Alice cloud admin:
juju add-permission user-alice@canonical.com administrator cloud-mycloud

# Add Bob and Cindy to the mygroup group:
juju add-permission user-bob@canonical.com member group-mygroup
juju add-permission user-cindy@canonical.com member group-mygroup

# Let everyone in group mygroup add models that will use resources from cloud my-cloud:
juju add-permission group-mygroup#member can-addmodel cloud-mycloud
```

|entity A | permission| entity B|
|-|-|-|
|{ref}`user tag or alternatives except for role assignee <user-tag>` |{ref}`role-permission-assignee`|{ref}`role tag <role-tag>`|
|{ref}`user tag or alternatives <user-tag>` |{ref}`group-permission-member`| {ref}`group tag <group-tag>`|
|{ref}`user tag or alternatives <user-tag>` |{ref}`controller-permission-audit-log-viewer`| {ref}`controller tag <controller-tag>`|
|{ref}`user tag or alternatives <user-tag>` |{ref}`controller-permission-administrator`| {ref}`controller tag <controller-tag>`|{ref}<-permission-
|{ref}`user tag or alternatives <user-tag>` |{ref}`cloud-permission-can-addmodel`| {ref}`cloud tag <cloud-tag>`|
|{ref}`user tag or alternatives <user-tag>` |{ref}`cloud-permission-administrator`| {ref}`cloud tag <cloud-tag>`|
|{ref}`user tag or alternatives <user-tag>` |{ref}`model-permission-reader`|{ref}`model tag <model-tag>`|
|{ref}`user tag or alternatives <user-tag>` |{ref}`model-permission-writer`|{ref}`model tag <model-tag>`|
|{ref}`user tag or alternatives <user-tag>` |{ref}`model-permission-administrator`|{ref}`model tag <model-tag>`|
|{ref}`user tag or alternatives <user-tag>` |{ref}`offer-permission-reader` | {ref}`offer tag <offer-tag>` |
|{ref}`user tag or alternatives <user-tag>` |{ref}`offer-permission-consumer` | {ref}`offer tag <offer-tag>`|
|{ref}`user tag or alternatives <user-tag>` |{ref}`offer-permission-administrator` | {ref}`offer tag <offer-tag>` |

For any given resource, permissions are currently hierarchical and some permissions are implicit -- e.g., given a cloud associated with a controller and a model associated with the cloud, a controller `administrator` entails cloud `administrator` entails cloud `can_addmodel`.

> See more: {doc}`juju add-permission <../reference/jaas-plugin>`

<!--
> - {ref}`user-tag`, {ref}`service-account-tag`, {ref}`role-tag`, {ref}`group-tag`, {ref}`controller-tag`, {ref}`cloud-tag`, {ref}`model-tag`, {ref}`offer-tag`
> - {ref}`list-of-service-account-permissions`, {ref}`list-of-role-permissions`, {ref}`list-of-group-permissions`, {ref}`list-of-controller-permissions`, {ref}`list-of-cloud-permissions`, {ref}`list-of-model-permissions`, {ref}`list-of-offer-permissions`
-->

<!--
# applicationoffer
("applicationoffer:some_offer#administrator", "consumer", "applicationoffer:some_offer")
("applicationoffer:some_offer#consumer", "reader", "applicationoffer:some_offer")
("group:some_group#member", "administrator", "applicationoffer:some_offer")
("group:some_group#member", "consumer", "applicationoffer:some_offer")
("group:some_group#member", "reader", "applicationoffer:some_offer")
("model:some_model", "model", "applicationoffer:some_offer")
("model:some_model#administrator", "administrator", "applicationoffer:some_offer")
("role:some_role#assignee", "administrator", "applicationoffer:some_offer")
("role:some_role#assignee", "consumer", "applicationoffer:some_offer")
("role:some_role#assignee", "reader", "applicationoffer:some_offer")
("user:*", "administrator", "applicationoffer:some_offer")
("user:*", "consumer", "applicationoffer:some_offer")
("user:*", "reader", "applicationoffer:some_offer")
("user:some_user", "administrator", "applicationoffer:some_offer")
("user:some_user", "consumer", "applicationoffer:some_offer")
("user:some_user", "reader", "applicationoffer:some_offer")

# cloud
("cloud:some_cloud#administrator", "can_addmodel", "cloud:some_cloud")
("controller:some_controller", "controller", "cloud:some_cloud")
("controller:some_controller#administrator", "administrator", "cloud:some_cloud")
("group:some_group#member", "administrator", "cloud:some_cloud")
("group:some_group#member", "can_addmodel", "cloud:some_cloud")
("role:some_role#assignee", "administrator", "cloud:some_cloud")
("role:some_role#assignee", "can_addmodel", "cloud:some_cloud")
("user:*", "administrator", "cloud:some_cloud")
("user:*", "can_addmodel", "cloud:some_cloud")
("user:some_user", "administrator", "cloud:some_cloud")
("user:some_user", "can_addmodel", "cloud:some_cloud")

# controller
("controller:some_controller#administrator", "administrator", "controller:some_controller")
("controller:some_controller#administrator", "audit_log_viewer", "controller:some_controller")
("controller:some_other_controller", "controller", "controller:some_controller")
("group:some_group#member", "administrator", "controller:some_controller")
("group:some_group#member", "audit_log_viewer", "controller:some_controller")
("role:some_role#assignee", "administrator", "controller:some_controller")
("role:some_role#assignee", "audit_log_viewer", "controller:some_controller")
("user:*", "administrator", "controller:some_controller")
("user:*", "audit_log_viewer", "controller:some_controller")
("user:some_user", "administrator", "controller:some_controller")
("user:some_user", "audit_log_viewer", "controller:some_controller")

# group
("group:some_other_group#member", "member", "group:some_group")
("user:*", "member", "group:some_group")
("user:some_user", "member", "group:some_group")

# model
("controller:some_controller", "controller", "model:some_model")
("controller:some_controller#administrator", "administrator", "model:some_model")
("group:some_group#member", "administrator", "model:some_model")
("group:some_group#member", "reader", "model:some_model")
("group:some_group#member", "writer", "model:some_model")
("model:some_model#administrator", "writer", "model:some_model")
("model:some_model#writer", "reader", "model:some_model")
("role:some_role#assignee", "administrator", "model:some_model")
("role:some_role#assignee", "reader", "model:some_model")
("role:some_role#assignee", "writer", "model:some_model")
("user:*", "administrator", "model:some_model")
("user:*", "reader", "model:some_model")
("user:*", "writer", "model:some_model")
("user:some_user", "administrator", "model:some_model")
("user:some_user", "reader", "model:some_model")
("user:some_user", "writer", "model:some_model")

# role
("group:some_group#member", "assignee", "role:some_role")
("user:*", "assignee", "role:some_role")
("user:some_user", "assignee", "role:some_role")

# serviceaccount
("group:some_group#member", "administrator", "serviceaccount:some_account")
("role:some_role#assignee", "administrator", "serviceaccount:some_account")
("user:*", "administrator", "serviceaccount:some_account")
("user:some_user", "administrator", "serviceaccount:some_account")

-->


(verify-a-permission)=
## Verify a permission

Given two entities A and B, to verify that there is a specific permission between them, run the `check-permission` command followed by the tag of A, the permission, and the tag of B. For example:

```text
juju check-permission user-alice@canonical.com administrator controller-aws-controller-1
```

> See more: {doc}`juju check-permission <../reference/jaas-plugin>`

(view-all-the-current-permissions)=
## View all the current permissions

To view all the current permissions, run the `list-permissions` command. For example:

```text
juju list-permissions [options]
```

> See more: {doc}`juju list-permissions <../reference/jaas-plugin>`


(remove-a-permission)=
## Remove a permission

Given two entities A and B and a pre-existing permission between them, to remove the permission, run the `remove-permission` command followed by the tag of A, the permission, and the tag of B. For example:

```text
juju remove-permission user-alice@canonical.com member group-mygroup
```

> See more: {ref}`view-all-the-current-permissions`, {doc}`juju remove-permission <../reference/jaas-plugin>`
