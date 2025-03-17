(controller.md)=
# Controller
> See first: {external+juju:ref}`Juju | Controller <controller>`


(controller-tag)=
## Controller tag
> See first: {ref}`tag`

A controller tag has the following format:

```text
controller-<controller name>
```

(controller-access-levels)=
## Controller access levels

(controller-access-levels-administrator)=
### `administrator`

Abilities: Can do anything that it is possible to do at the level of a controller. This grants permissions to all resources that inherit from controller access.

(controller-access-levels-audit-log-viewer)=
### `audit_log_viewer`

Abilities: Can read audit logs.
