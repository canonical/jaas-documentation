(service-account)=
# Service account
> See also: {manage-service-accounts}

In JAAS, a service account is a machine identity used for non-human interactions with a Juju cotnroller.

A service account identifier will vary depending on the identity provider.

(service-account-tag)=
## Service account tag
> See first: {ref}`tag`

A service account tag has the following format:

```text
serviceaccount-<identifier>
```

where `<identifier>` is the name/id of the service account as prvoided by your IdP.

```{tip}
A `serviceaccount-<identifier>` type tag is only useful for providing permissions over administration of a service account.

In all other cases treat service accounts as users and use a {ref}`user tag <user-tag>` to grant them access to resources.
```

(list-of-service-account-relations)=
## List of service account relations
> See first: {ref}`relation`


(service-account-relation-administrator)=
### `administrator`

Abilities: Can do anything that it is possible to do at the level of a service account. Used to manage the credentials of a service account.
