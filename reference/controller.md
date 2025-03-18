(controller.md)=
# Controller
> See first: {external+juju:ref}`Juju | Controller <controller>`
>
> See also: {ref}`manage-juju-controllers`

(controller-tag)=
## Controller tag
> See first: {ref}`tag`

A controller tag has the following format:

```text
controller-<controller name>
```

(controller-relation)=
## Controller relation

A controller relation is a {ref}`relation <relation>` that describes permissions on a controller.

(list-of-controller-relations)=
### List of controller relations
> See first: {ref}`relation`

(controller-relation-administrator)=
#### `administrator`

Abilities: Can do anything that it is possible to do at the level of a controller. This grants permissions to all resources that inherit from controller access.

(controller-relation-audit-log-viewer)=
#### `audit_log_viewer`

Abilities: Can read audit logs.