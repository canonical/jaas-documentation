(group)=
# Group

In JAAS, a group is a collection of users, services accounts, and/or groups.

A group is referenced by name (which is internally matched to a unique ID).

(group-tag)=
## Group tag
> See first: {ref}`tag`

A group tag has the following format:

```text
group-<group id>
```

where `group id` represents the unique identifier of the group.


(list-of-group-relations)=
## List of group relations
> See first: {ref}`relation`

(group-relation-member)=
### `member`

Abilities: Shares the group's access level to Juju resources and JIMM logs.

