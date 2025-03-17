(manage-relations)=
# Manage relations
> See also: {ref}`relation`

(add-a-relation)=
## Add a relation

Given two entities A and B, to add a relation between run the `auth relation add` command followed by the tag of A, the access level you want to give A from the list supported by B, and the tag of B. For example, to add a relation where you make user `alice@canonical.com` a `member` of the `mygroup` group:

```text
jimmctl auth relation add user-alice@canonical.com member group-mygroup
```

> See more:
> - {ref}`tag`
> - {ref}`jimmctl auth relation add <summary-10>`
> - {ref}`controller-access-levels`, {ref}`cloud-access-levels`, {ref}`model-access-levels`, {ref}`offer-access-levels`, {ref}`service-account-access-levels`, {ref}`role-access-levels`, {ref}`group-access-levels`


(verify-a-relation)=
## Verify a relation

Given two entities A and B, to verify that there is a relation between them with a specific access level, run the `auth relation check` command followed by the tag of A, the access level, and the tag of B. For example:

```text
jimmctl auth relation check user-alice@canonical.com administrator controller-aws-controller-1
```

> See more: {ref}`jimmctl auth relation check <summary-11>`

(view-all-the-current-relations)=
## View all the current relations

To view all the current relations, run the `auth relation list` command. For example:

```text
jimmctl auth relation list [options]
```

> See more: {ref}`jimmctl auth relation list <summary-12>`


(remove-a-relation)=
## Remove a relation

Given two entities A and B and a pre-existing relation between them, to remove the relation, run the `auth relation remove` command followed by the tag of A, the access level that A has to B, and the tag of B. For example:

```text
jimmctl auth relation remove user-alice@canonical.com member group-mygroup
```

> > See more: {ref}`view-all-the-current-relations`, {ref}`jimmctl auth relation remove <summary-13>`