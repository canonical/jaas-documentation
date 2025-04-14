(manage-relations)=
# Manage relations
> See also: {ref}`relation`

(add-a-relation)=
## Add a relation

Given two entities A and B, to add a relation between them run the `auth relation add` command followed by the tag of A, the desired B relation, and the tag of B. For example, to add a relation where you make user `alice@canonical.com` a `member` of the `mygroup` group:

```text
juju add-permission user-alice@canonical.com member group-mygroup
```

> See more:
> - {ref}`juju add-permission <summary-10>`
> - {ref}`controller-tag`, {ref}`cloud-tag`, {ref}`model-tag`, {ref}`offer-tag`, {ref}`user-tag`, {ref}`service-account-tag`, {ref}`role-tag`, {ref}`group-tag`
> - {ref}`list-of-controller-relations`, {ref}`list-of-cloud-relations`, {ref}`list-of-model-relations`, {ref}`list-of-offer-relations`, {ref}`list-of-service-account-relations`, {ref}`list-of-role-relations`, {ref}`list-of-group-relations`


(verify-a-relation)=
## Verify a relation

Given two entities A and B, to verify that there is a specific relation between them, run the `auth relation check` command followed by the tag of A, the relation, and the tag of B. For example:

```text
juju check-permission user-alice@canonical.com administrator controller-aws-controller-1
```

> See more: {ref}`juju check-permission <summary-11>`

(view-all-the-current-relations)=
## View all the current relations

To view all the current relations, run the `auth relation list` command. For example:

```text
juju list-permissions [options]
```

> See more: {ref}`juju list-permissions <summary-12>`


(remove-a-relation)=
## Remove a relation

Given two entities A and B and a pre-existing relation between them, to remove the relation, run the `auth relation remove` command followed by the tag of A, the relation, and the tag of B. For example:

```text
juju remove-permission user-alice@canonical.com member group-mygroup
```

> > See more: {ref}`view-all-the-current-relations`, {ref}`juju remove-permission <summary-13>`
