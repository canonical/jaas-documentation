(manage-service-accounts)=
# Manage service accounts
> See also: {ref}`service-account`

(control-user-access-to-a-service-account)=
## Control user access to a service account

To grant a (collection of) user(s) access to a service account, add an `administrator` permission between the user(s) and the service account. For example:

For example:

```text
juju add-permission serviceaccount-myserviceaccount administrator serviceaccount-myotherserviceaccount
```

> See more: {ref}`manage-permissions`

<!--
# serviceaccount
("group:some_group#member", "administrator", "serviceaccount:some_account")
("role:some_role#assignee", "administrator", "serviceaccount:some_account")
("user:*", "administrator", "serviceaccount:some_account")
("user:some_user", "administrator", "serviceaccount:some_account")
-->
