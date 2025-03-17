(manage-juju-controllers)=
# Manage Juju controllers
> Who: JIMM controller admin
>
> See also: {ref}`controller`

<!--
ADD:
jimmctl controller-info (part of the process for adding a Juju controller to JIMM), add-controller
jimmctl controllers
add-cloud-to-controller
jimmctl remove-cloud-from-controller
jimmctl remove-controller
jimmctl set-controller-deprecated
-->


(manage-access-to-a-juju-controller)=
## Manage access to a Juju controller

**Grant access to a Juju controller.** Given an entity A and a controller, to grant A access to the controller, add a relation between A and the controller, specifying the desired access level.

> See more: {ref}`add-a-relation`

**Revoke access to a Juju controller.** Given an entity A and a controller, to revoke A's access to the controller, remove the relation that grants that access.

> See more: {ref}`remove-a-relation`

