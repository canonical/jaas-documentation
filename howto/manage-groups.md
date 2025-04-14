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

To add a new group to your JIMM controller, use the `auth group add` command followed by the name you want to assign to the group. For example:

```text
juju add-group A
```

> See more: {ref}`juju add-group <summary-5>`

(view-all-the-current-groups)=
## View all the current groups

To view all the current groups, run the `auth group list` command. For example:

```text
juju list-groups [options]
```

> See more: {ref}`juju list-groups <summary-6>`

(manage-an-entitys-relation-to-a-group)=
## Manage an entity's relation to a group

See {ref}`manage-relations`.

(manage-a-groups-relation-to-an-entity)=
## Manage a group's relation to an entity

See {ref}`manage-relations`.

(rename-a-group)=
## Rename a group

To rename a group, run the `auth group rename` command followed by the old name and the new name. For example:

```text
juju rename-group TeamA TeamB
```

> See more: {ref}`juju rename-group <summary-8>`

(remove-a-group)=
## Remove a group

To remove a group from a JIMM controller, run the `auth group remove` command followed by the name of the group. For example:

```text
juju remove-group TeamB
```

> See more: {ref}`juju remove-group <summary-7>`
