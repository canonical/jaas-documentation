(manage-roles)=
# Manage roles
> Who: JIMM controller admin
>
> See also: {ref}`role`

````{dropdown} Preview an example workflow

```text
# Create a role
jimmctl auth role add myrole

# Verify that the role has been created successfully:
jimmctl auth role list

# Give the role admin access to a model:
jimmctl auth relation add role-model-admin#assignee administrator model-bob@canonical.com/foo

# Rename the role to better match its function:
jimmctl auth role rename model-admin

# Grant Alice access to the role
jimmctl auth relation add user-alice@canonical.com assignee role-model-admin

# Verify that Alice's access to the role has been granted successfully:
jimmctl auth relation check user-alice@canonical.com administrator model-bob@canonical.com/foo
```
````

(add-a-role)=
## Add a role

To add a new role to your JIMM controller, use the `auth role add` command followed by the name you want to assign to the role. For example:

```text
jimmctl auth role add model-admin
```

> See more: {ref}`jimmctl auth role add <summary-15>`

(view-all-the-current-roles)=
## View all the current roles

To view all the current roles, run the `auth role list` command. For example:

```text
jimmctl auth role list [options]
```

> See more: {ref}`jimmctl auth role list <summary-16>`


(manage-access-to-a-role)=
## Manage access to a role

**Grant access to a role.** Given an entity A and a role, to grant A access to the role, add a relation between A and the role, specifying the desired access level.

> See more: {ref}`add-a-relation`

**Revoke access to a role.** Given an entity A and a role, to revoke A's access to the role, remove the relation that grants that access.

> See more: {ref}`remove-a-relation`

(manage-a-roles-access)=
## Manage a role’s access to a controller, cloud, model, or offer

> See more: {ref}`manage-access-to-a-controller`, {ref}`manage-access-to-a-cloud`, {ref}`manage-access-to-a-model`, {ref}`manage-access-to-an-offer`

(rename-a-role)=
## Rename a role

To rename a role, run the `auth role rename` command followed by the old name and the new name. For example:

```text
jimmctl auth role rename model-admin model-writer
```

> See more: {ref}`jimmctl auth role rename <summary-18>`

(remove-a-role)=
## Remove a role

To remove a role from a JIMM controller, run the `auth role remove` command followed by the name of the role. For example:

```text
jimmctl auth role remove model-admin
```

> See more: {ref}`jimmctl auth role remove <summary-17>`
