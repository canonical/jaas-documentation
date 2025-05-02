(manage-users)=
# Manage users
> See first: {ref}`user`

## Set up a new user

Configure your JIMM controller to have a DNS address and share it with the user.

> See more: [Charmhub | `juju-jimm-k8s` > Configurations > `dns-name`](https://charmhub.io/juju-jimm-k8s/configurations#dns-name)

Add a Juju controller to JAAS.

Add the user to JAAS.

> See more: {ref}`manage-a-users-relation-to-an-entity`

Assuming the user already has the `juju` CLI client installed, get the user to log in to JIMM using the DNS address. For example:

```text
juju login test-jimm.localhost:443 -c jaas
```

At this point `juju controllers` should show the `jaas` controller and commands like `juju models` should work work. However, to perform any meaningful operations, the user will also need access to a cloud by adding a cloud (or being granted access to one via `juju grant-cloud`), adding cloud credentials, and using `juju update-credentials` with the `--controller jimm` flag to make their credentials available to JIMM (which will store them in Vault). For example, assuming the `localhost` cloud:

```text
juju update-credentials localhost --controller jimm
```

At this point the user can start doing whatever the permissions you've granted them allow them to do, for example, add a model and deploy applications to it.

(manage-permissions-related-to-a-user)=
## Manage permissions related to a user

```{note}
This permissions mechanism supplements the Juju way of granting a user access to controllers, clouds, models, and offers, and can be used in addition to it. See more: [Juju | Manage a user's access level](https://canonical-juju.readthedocs-hosted.com/en/3.6/user/howto/manage-users/) or [Terraform Provider Juju | Manage a user's access level](https://canonical-terraform-provider-juju.readthedocs-hosted.com/en/latest/howto/manage-users/).
```

Given a user and an entity B, to grant the user permissions on B run the `add-permission` command followed by the tag of the user, the desired B-supported permission, and the tag of B, where the possible (user, permission, B) combinations are:

```
("user:*", "administrator", "applicationoffer:some_offer")
("user:*", "administrator", "cloud:some_cloud")
("user:*", "administrator", "controller:some_controller")
("user:*", "administrator", "model:some_model")
("user:*", "administrator", "serviceaccount:some_account")
("user:*", "assignee", "role:some_role")
("user:*", "audit_log_viewer", "controller:some_controller")
("user:*", "can_addmodel", "cloud:some_cloud")
("user:*", "consumer", "applicationoffer:some_offer")
("user:*", "member", "group:some_group")
("user:*", "reader", "applicationoffer:some_offer")
("user:*", "reader", "model:some_model")
("user:*", "writer", "model:some_model")
("user:some_user", "administrator", "applicationoffer:some_offer")
("user:some_user", "administrator", "cloud:some_cloud")
("user:some_user", "administrator", "controller:some_controller")
("user:some_user", "administrator", "model:some_model")
("user:some_user", "administrator", "serviceaccount:some_account")
("user:some_user", "assignee", "role:some_role")
("user:some_user", "audit_log_viewer", "controller:some_controller")
("user:some_user", "can_addmodel", "cloud:some_cloud")
("user:some_user", "consumer", "applicationoffer:some_offer")
("user:some_user", "member", "group:some_group")
("user:some_user", "reader", "applicationoffer:some_offer")
("user:some_user", "reader", "model:some_model")
("user:some_user", "writer", "model:some_model")
```

For example:

```text
juju add-permission user-alice@canonical.com member group-mygroup
```

> See more: {ref}`manage-permissions`
