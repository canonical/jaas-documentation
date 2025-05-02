(offer)=
# Offer
> See first: {external+juju:ref}`Juju | Offer <offer>`
>
> See also: {ref}`manage-offers`

(offer-tag)=
## Offer tag

An application offer tag has the following format:

```text
applicationoffer-<controller name>/<model name>.<offer name>
```

where `<controller name>` specifies name of the controller on which the model
is running, `<model name>` specifies name of the model in which the application
offer was created and `<offer name>` specifies the name of the application offer.

(offer-relation)=
## Offer relation

An offer relation is a {ref}`relation <relation>` that describes permissions on a offer.

(list-of-offer-relations)=
## List of offer relations

(offer-relation-administrator)=
### `administrator`

Abilities: Can do anything that it is possible to do at the level of an offer.

(offer-relation-consumer)=
### `consumer`

Abilities: Can relate an application to the offer.

(offer-relation-reader)=
### `reader`

Abilities: Can view offers during a search with `juju find-offers`.