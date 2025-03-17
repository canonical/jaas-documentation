(role)=
# Role

> See also: {ref}`manage-roles`

In JAAS, a role is a property of an entity that describes what they can do in a JIMM controller.

(role-tag)=
## Role tag
> See first: {ref}`tag`

A role tag has the following format:

```text
role-<role name>
role-<role id>
```

where `role id` represents the unique identifier of the role.


(role-access-levels)=
## Role access levels

(role-access-levels-assignee)=
### `assignee`

Abilities: Shares the role's access level to Juju resources and JIMM logs.

