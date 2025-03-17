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


(manage-access-to-a-group)=
## Manage access to a group

**Grant access to a group.** Given an entity A and a group, to grant A access to the group, add a relation between A and the group, specifying the desired access level.

> See more: {ref}`add-a-relation`

**Revoke access to a group.** Given an entity A and a group, to revoke A's access to the group, remove the relation that grants that access.

> See more: {ref}`remove-a-relation`

(manage-a-groups-access)=
## Manage a group’s access to a controller, cloud, model, offer, or group

> See more: {ref}`manage-access-to-a-controller`, {ref}`manage-access-to-a-cloud`, {ref}`manage-access-to-a-model`, {ref}`manage-access-to-an-offer`, {ref}`manage-access-to-a-group`


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
