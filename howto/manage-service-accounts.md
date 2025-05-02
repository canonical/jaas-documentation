(manage-service-accounts)=
# Manage service accounts
> See also: {ref}`service-account`

(manage-permissions-related-to-a-service account)=
## Manage permissions related to a service account

Given an entity A and a service account, to grant A permissions on the service account run the `add-permission` command followed by the tag of A, the desired service account permission, and the tag of the service account, where the possible (A, permission, service account) combinations are:

```
("group:some_group#member", "administrator", "serviceaccount:some_account")
("role:some_role#assignee", "administrator", "serviceaccount:some_account")
("user:*", "administrator", "serviceaccount:some_account")
("user:some_user", "administrator", "serviceaccount:some_account")
```

For example:

```text
juju add-permission user-alice@canonical.com administrator serviceaccount-myserviceaccount
```

Given a service account and an entity B, to grant the service account permissions on B run the `add-permission` command followed by the tag of the service account, the desired B-supported permission, and the tag of B, where the possible (service account, permission, B) combinations are:

```
("serviceaccount:some_account", "administrator", "serviceaccount:some_account")
```

For example:

```text
juju add-permission serviceaccount-myserviceaccount administrator serviceaccount-myotherserviceaccount
```

> See more: {ref}`manage-permissions`