(manage-groups)=
# Manage groups
> Who: JIMM controller admin
>
> See also: {ref}`group`

````{dropdown} Preview an example workflow
```text
# Create a group:
jimmctl auth group add A

# Verify that the group has been created successfully:
jimmctl auth group list

# Give the members of the group write access to test-model-1:
jimmctl auth relation add group-B#members writer model-test-ctl-1/test-model-1

# Rename the role to something more suitable:
jimmctl auth group rename model-writers

# Add users to the group:
jimmctl auth relation add user-alice@canonical.com member group-A
jimmctl auth relation add user-bob@canonical.com member group-B

# Verify that user Alice has indeed inherited the group's write access to test-model-1:
jimmctl auth relation check user-alice@canonical.com writer model-test-ctl-1/test-model-1

# Create another group B and make members of group A also members of group B:
jimmctl auth relation add group-A#member member group-B
...
```
````

(add-a-group)=
## Add a group

To add a new group to your JIMM controller, use the `auth group add` command followed by the name you want to assign to the group. For example:

```text
jimmctl auth group add A
```

> See more: {ref}`jimmctl auth group add <summary-5>`

(view-all-the-current-groups)=
## View all the current groups

To view all the current groups, run the `auth group list` command. For example:

```text
jimmctl auth group list [options]
```

> See more: {ref}`jimmctl auth group list <summary-6>`

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
jimmctl auth group rename TeamA TeamB
```

> See more: {ref}`jimmctl auth group rename <summary-8>`

(remove-a-group)=
## Remove a group

To remove a group from a JIMM controller, run the `auth group remove` command followed by the name of the group. For example:

```text
jimmctl auth group remove TeamB
```

> See more: {ref}`jimmctl auth group remove <summary-7>`
