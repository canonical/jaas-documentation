(manage-offers)=
# Manage offers
> See also: {ref}`offer`


(control-user-access-to-an-offer)=
## Control user access to an offer

To grant a (collection of) user(s) access to an application offer, add a `reader`, `consumer`, or `administrator` permission between the user(s) and the offer. For example:

For example:

```text
# Let Alice consume offer myoffer:
juju add-permission user-alice@canonical.com consumer applicationoffer-mycontroller/mymodel.myoffer
```

> See more: {ref}`manage-permissions`

<!--
# applicationoffer
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
-->

