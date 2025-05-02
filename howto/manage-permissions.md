(manage-permissions)=
# Manage permissions
> See first: {external+juju:ref}`Juju access levels <user-access-levels>`
>
> See also: {ref}`jaas-authorization`

(add-a-permission)=
## Add a permission

Given two entities A and B, to add a permission between them run the `add-permission` command followed by the tag of A, the desired B-supported permission, and the tag of B. For example:

```text
juju add-permission user-alice@canonical.com member group-mygroup
```

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
