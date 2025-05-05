(manage-permissions)=
# Manage permissions
> See first: {external+juju:ref}`Juju | Juju access levels <user-access-levels>`
>
> See also: {ref}`jaas-authorization`

(add-a-permission)=
## Add a permission

Given two entities A and B, to add a permission between them run the `add-permission` command followed by the tag of A, the desired B-supported permission, and the tag of B. For example:

```text
juju add-permission group-mygroup#member can-addmodel model-mymodel
```


````{dropdown} View the full list of possible combinations of (A, permission, B), grouped by A

```text
# application offer
("applicationoffer:some_offer#administrator", "consumer", "applicationoffer:some_offer")
("applicationoffer:some_offer#consumer", "reader", "applicationoffer:some_offer")

# cloud
("cloud:some_cloud#administrator", "can_addmodel", "cloud:some_cloud")

# controller
("controller:some_controller", "controller", "cloud:some_cloud")
("controller:some_controller", "controller", "controller:some_controller")
("controller:some_controller#administrator", "administrator", "cloud:some_cloud")
("controller:some_controller#administrator", "administrator", "controller:some_controller")
("controller:some_controller#administrator", "audit_log_viewer", "controller:some_controller")
("controller:some_other_controller", "controller", "controller:some_controller")

# group
("group:some_group#member", "administrator", "applicationoffer:some_offer")
("group:some_group#member", "administrator", "cloud:some_cloud")
("group:some_group#member", "administrator", "controller:some_controller")
("group:some_group#member", "administrator", "model:some_model")
("group:some_group#member", "administrator", "serviceaccount:some_account")
("group:some_group#member", "assignee", "role:some_role")
("group:some_group#member", "audit_log_viewer", "controller:some_controller")
("group:some_group#member", "can_addmodel", "cloud:some_cloud")
("group:some_group#member", "consumer", "applicationoffer:some_offer")
("group:some_group#member", "member", "group:some_group")
("group:some_group#member", "reader", "applicationoffer:some_offer")
("group:some_group#member", "reader", "model:some_model")
("group:some_group#member", "writer", "model:some_model")
("group:some_other_group#member", "member", "group:some_group")

# model
("model:some_model", "model", "applicationoffer:some_offer")
("model:some_model#administrator", "administrator", "applicationoffer:some_offer")
("model:some_model#administrator", "writer", "model:some_model")
("model:some_model#writer", "reader", "model:some_model")

# role
("role:some_role#assignee", "administrator", "applicationoffer:some_offer")
("role:some_role#assignee", "administrator", "cloud:some_cloud")
("role:some_role#assignee", "administrator", "controller:some_controller")
("role:some_role#assignee", "administrator", "model:some_model")
("role:some_role#assignee", "administrator", "serviceaccount:some_account")
("role:some_role#assignee", "assignee", "role:some_role")
("role:some_role#assignee", "can_addmodel", "cloud:some_cloud")
("role:some_role#assignee", "consumer", "applicationoffer:some_offer")
("role:some_role#assignee", "reader", "applicationoffer:some_offer")
("role:some_role#assignee", "reader", "model:some_model")
("role:some_role#assignee", "writer", "model:some_model")

# service account
("serviceaccount:some_account", "administrator", "serviceaccount:some_account")

# user
("user:*", "administrator", "applicationoffer:some_offer")
("user:*", "administrator", "cloud:some_cloud")
("user:*", "administrator", "controller:some_controller")
("user:*", "administrator", "model:some_model")
("user:*", "administrator", "serviceaccount:some_account")
("user:*", "assignee", "role:some_role")
("user:*", "audit_log_viewer", "controller:some_controller")
("user:*", "can_addmodel", "cloud:some_cloud")
("user:*", "consumer", "applicationoffer:some_offer")
("user:*", "member", "group:some_group")
("user:*", "reader", "applicationoffer:some_offer")
("user:*", "reader", "model:some_model")
("user:*", "writer", "model:some_model")
("user:some_user", "administrator", "applicationoffer:some_offer")
("user:some_user", "administrator", "cloud:some_cloud")
("user:some_user", "administrator", "controller:some_controller")
("user:some_user", "administrator", "model:some_model")
("user:some_user", "administrator", "serviceaccount:some_account")
("user:some_user", "assignee", "role:some_role")
("user:some_user", "audit_log_viewer", "controller:some_controller")
("user:some_user", "can_addmodel", "cloud:some_cloud")
("user:some_user", "consumer", "applicationoffer:some_offer")
("user:some_user", "member", "group:some_group")
("user:some_user", "reader", "applicationoffer:some_offer")
("user:some_user", "reader", "model:some_model")
("user:some_user", "writer", "model:some_model")

```

````

````{dropdown} View the full list of possible combinations of (A, permission, B), grouped by B
```
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

```
````


> See more:
> - {doc}`juju add-permission <../reference/jaas-plugin>`
> - {ref}`controller-tag`, {ref}`cloud-tag`, {ref}`model-tag`, {ref}`offer-tag`, {ref}`user-tag`, {ref}`service-account-tag`, {ref}`role-tag`, {ref}`group-tag`
> - {ref}`list-of-controller-permissions`, {ref}`list-of-cloud-permissions`, {ref}`list-of-model-permissions`, {ref}`list-of-offer-permissions`, {ref}`list-of-service-account-permissions`, {ref}`list-of-role-permissions`, {ref}`list-of-group-permissions`




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
