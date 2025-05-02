(manage-groups)=
# Manage groups
> Who: JIMM controller admin
>
> See also: {ref}`group`

```{note}
This guide assumes you have the `jaas` plugin installed.
See {doc}`here <../explanation/jaas-plugin>` for more information.
```

````{dropdown} Preview an example workflow
```text
# Create a group:
juju add-group A

# Verify that the group has been created successfully:
juju list-groups

# Give the members of the group write access to test-model-1:
juju add-permission group-B#members writer model-test-ctl-1/test-model-1

# Rename the role to something more suitable:
juju rename-group model-writers

# Add users to the group:
juju add-permission user-alice@canonical.com member group-A
juju add-permission user-bob@canonical.com member group-B

# Verify that user Alice has indeed inherited the group's write access to test-model-1:
juju check-permission user-alice@canonical.com writer model-test-ctl-1/test-model-1

# Create another group B and make members of group A also members of group B:
juju add-permission group-A#member member group-B
...
```
````

(add-a-group)=
## Add a group

To add a new group to your JIMM controller, use the `add-group` command followed by the name you want to assign to the group. For example:

```text
juju add-group A
```

> See more: {doc}`juju add-group <../reference/jaas-plugin>`

(view-all-the-current-groups)=
## View all the current groups

To view all the current groups, run the `list-groups` command. For example:

```text
juju list-groups [options]
```

> See more: {doc}`juju list-groups <../reference/jaas-plugin>`


(manage-permissions-related-to-a-group)=
## Manage permissions related to a group

Given an entity A and a group, to grant A permissions on the group run the `add-permission` command followed by the tag of A, the desired group permission, and the tag of the group, where the possible (A, permission, group) combinations are:

```
("group:some_other_group#member", "member", "group:some_group")
("user:*", "member", "group:some_group")
("user:some_user", "member", "group:some_group")
```

For example:

```text
juju add-permission user-alice@canonical.com member group-mygroup
```

Given a group and an entity B, to grant the group permissions on B run the `add-permission` command followed by the tag of the group, the desired group permission, and the tag of B, where the possible (group, permission, B) combinations are:

```
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
```

For example:

```text
juju add-permission group-mygroup#member member group-mynewgroup
```

> See more: {ref}`manage-permissions`.


(rename-a-group)=
## Rename a group

To rename a group, run the `rename-group` command followed by the old name and the new name. For example:

```text
juju rename-group TeamA TeamB
```

> See more: {doc}`juju rename-group <../reference/jaas-plugin>`

(remove-a-group)=
## Remove a group

To remove a group from a JIMM controller, run the `remove-group` command followed by the name of the group. For example:

```text
juju remove-group TeamB
```

> See more: {doc}`juju remove-group <../reference/jaas-plugin>`
