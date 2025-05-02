(manage-clouds)=
# Manage clouds
> See first: {ref}`cloud`

(manage-permissions-related-to-a-cloud)=
## Manage permissions related to a cloud

Given an entity A and a cloud, to grant A permissions on the cloud run the `add-permission` command followed by the tag of A, the desired cloud permission, and the tag of the cloud, where the possible (A, permission, cloud) combinations are:

```
("cloud:some_cloud#administrator", "can_addmodel", "cloud:some_cloud")
("controller:some_controller", "controller", "cloud:some_cloud")
("controller:some_controller#administrator", "administrator", "cloud:some_cloud")
("group:some_group#member", "administrator", "cloud:some_cloud")
("group:some_group#member", "can_addmodel", "cloud:some_cloud")
("role:some_role#assignee", "administrator", "cloud:some_cloud")
("role:some_role#assignee", "can_addmodel", "cloud:some_cloud")
("user:*", "administrator", "cloud:some_cloud")
("user:*", "can_addmodel", "cloud:some_cloud")
("user:some_user", "administrator", "cloud:some_cloud")
("user:some_user", "can_addmodel", "cloud:some_cloud")
```

For example:

```text
juju add-permission user-alice@canonical.com administrator cloud-mycloud
```

> See more: {ref}`manage-permissions`.