(manage-roles)=
# Manage roles
> Who: JIMM controller admin
>
> See also: {ref}`role`

(add-a-role)=
## Add a role

To add a new role to your JIMM controller, use the `auth role add` command followed by the name you want to assign to the role. For example:

```text
jimmctl auth role add model-admin
```

> See more: {ref}`jimmctl auth role add <summary-15>`

(view-all-the-current-roles)=
## View all the current roles

(manage-access-to-a-role)=
## Manage access to a role

To grant access to a role, run the `auth relation add` command followed by the name of an entity you've defined earlier (user, service account, or group, in their tag notation), the desired role access level (currently just `assignee`), and the name of a role you've defined earlier. For example, assuming a predefined user `alice@canonical.com` and a predefine role `model-admin`, run:

```text
jimmctl auth relation add user-alice@canonical.com assignee role-model-admin
```

> See more: {ref}`jimmctl auth relation add <summary-10>`, {ref}`tag`, {ref}`role-access-levels`




a user call
- `alice@canonical.com`


Next, let us create a role. Run:

```text
jimmctl auth role add model-admin
```

To assign this role to Alice run:



> See more: [JAAS | Role access levels](https://canonical-jaas-documentation.readthedocs-hosted.com/en/latest/reference/authorisation_model/#role)

(manage-a-roles-access)=
## Manage a role’s access to a controller, cloud, model, offer, or group

(rename-a-role)=
## Rename a role

(remove-a-role)=
## Remove a role


## Role management

For this part of the tutorial we will assume the following user exists in an organisation:

- `alice@canonical.com`

Next, let us create a role. Run:

```text
jimmctl auth role add model-admin
```

To assign this role to Alice run:

```text
jimmctl auth relation add user-alice@canonical.com assignee role-model-admin
```

Now Alice is assigned the `model-admin` role.
Note that the role has no permissions yet (see `Granting access to roles`).

To view all available roles run:

```text
jimmctl auth role list
```

we will see the `model-admin` role.

Renaming a role **does not** affect role membership or any access rights a role
might already have in JAAS.

To rename the role, run (the remainder of this doc will assume the name remains as `model-admin`):

```text
jimmctl auth role rename model-admin model-writer
```

To remove role `model-admin` from JAAS, run:

```text
jimmctl auth role remove model-admin
```

## Granting access to roles

Now that we know how to manage roles and roles membership let's take a look
at how we can grant roles access to resources in JIMM.

This section assumes the `model-admin` role was created and `alice@canonical.com`
was assigned the role.

Because roles are currently identical to groups, our `model-admin` role needs
to be assigned access to individual resources.

Assuming a model `bob@canonical.com/foo` exists, run:

```text
jimmctl auth relation add role-model-admin#assignee administrator model-bob@canonical.com/foo
```

Now let us check if `alice@canonical.com` has administrator access to the model by running:

```text
jimmctl auth relation check user-alice@canonical.com administrator model-bob@canonical.com/foo
```

We should get a positive result since `alice@canonical.com` is member of role `model-admin`.

To remove role `model-admin`'s access to the model we can run:

```text
jimmctl auth relation remove role-model-admin#assignee administrator model-bob@canonical.com/foo
```

Finally, to list the users who have been assigned the `model-admin` role we can run:

```text
jimmctl auth relation list --target role-model-admin
```

## Conclusion

This tutorial taught you the basics of role management in JAAS.

