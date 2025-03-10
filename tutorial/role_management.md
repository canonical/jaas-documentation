# Role management

Introduction

```{tip}
Roles are currently identical to groups in functionality.
This tutorial is a summarised version of {doc}`./group_management`.
```


JAAS provides role management capabilities, this allows JAAS
administrators to assign roles to users granting them access to resources.

In this tutorial we will show you how to manage roles in JAAS.

## Prerequisites

For this tutorial you will need the following:

- At least one controller connected to JIMM  (see {doc}`../howto/add_controller`)
- `jimmctl` CLI (installed via [Snap](https://snapcraft.io/jimmctl))

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

