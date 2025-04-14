(manage-relations)=
# Manage relations
> See also: {ref}`relation`

(add-a-relation)=
## Add a relation

Given two entities A and B, to add a relation between them run the `add-permission` command followed by the tag of A, the desired B relation, and the tag of B. For example, to add a relation where you make user `alice@canonical.com` a `member` of the `mygroup` group:

```text
juju add-permission user-alice@canonical.com member group-mygroup
```

> See more:
> - {doc}`juju add-permission <../reference/jaas-plugin>`
> - {ref}`controller-tag`, {ref}`cloud-tag`, {ref}`model-tag`, {ref}`offer-tag`, {ref}`user-tag`, {ref}`service-account-tag`, {ref}`role-tag`, {ref}`group-tag`
> - {ref}`list-of-controller-relations`, {ref}`list-of-cloud-relations`, {ref}`list-of-model-relations`, {ref}`list-of-offer-relations`, {ref}`list-of-service-account-relations`, {ref}`list-of-role-relations`, {ref}`list-of-group-relations`


(verify-a-relation)=
## Verify a relation

Given two entities A and B, to verify that there is a specific relation between them, run the `check-permission` command followed by the tag of A, the relation, and the tag of B. For example:

```text
juju check-permission user-alice@canonical.com administrator controller-aws-controller-1
```

> See more: {doc}`juju check-permission <../reference/jaas-plugin>`

(view-all-the-current-relations)=
## View all the current relations

To view all the current relations, run the `list-permissions` command. For example:

```text
juju list-permissions [options]
```

> See more: {doc}`juju list-permissions <../reference/jaas-plugin>`


(remove-a-relation)=
## Remove a relation

Given two entities A and B and a pre-existing relation between them, to remove the relation, run the `remove-permission` command followed by the tag of A, the relation, and the tag of B. For example:

```text
juju remove-permission user-alice@canonical.com member group-mygroup
```

> > See more: {ref}`view-all-the-current-relations`, {doc}`juju remove-permission <../reference/jaas-plugin>`
