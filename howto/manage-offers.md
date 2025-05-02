(manage-offers)=
# Manage application offers

(manage-permissions-related-to-an-offer)=
## Manage permissions related to an offer

Given an entity A and an application offer, to grant A permissions on the offer run the `add-permission` command followed by the tag of A, the desired offer permission, and the tag of the offer, where the possible (A, permission, offer) combinations are:

```
("applicationoffer:some_offer#administrator", "consumer", "applicationoffer:some_offer")
("applicationoffer:some_offer#consumer", "reader", "applicationoffer:some_offer")
("group:some_group#member", "administrator", "applicationoffer:some_offer")
("group:some_group#member", "consumer", "applicationoffer:some_offer")
("group:some_group#member", "reader", "applicationoffer:some_offer")
("model:some_model", "model", "applicationoffer:some_offer")
("model:some_model#administrator", "administrator", "applicationoffer:some_offer")
("role:some_role#assignee", "administrator", "applicationoffer:some_offer")
("role:some_role#assignee", "consumer", "applicationoffer:some_offer")
("role:some_role#assignee", "reader", "applicationoffer:some_offer")
("user:*", "administrator", "applicationoffer:some_offer")
("user:*", "consumer", "applicationoffer:some_offer")
("user:*", "reader", "applicationoffer:some_offer")
("user:some_user", "administrator", "applicationoffer:some_offer")
("user:some_user", "consumer", "applicationoffer:some_offer")
("user:some_user", "reader", "applicationoffer:some_offer")
```

For example:

```text
juju add-permission user-alice@canonical.com consumer applicationoffer-mycontroller/mymodel.myoffer
```

> See more: {ref}`manage-permissions`
