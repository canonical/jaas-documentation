(offer)=
# Offer
> See first: {external+juju:ref}`Juju | Offer <offer>`

(offer-tag)=
## Offer tag
> See first: {ref}`tag`

An application offer tag has the following format:

```text
applicationoffer-<controller name>/<model name>.<offer name>
```

where `<controller name>` specifies name of the controller on which the model
is running, `<model name>` specifies name of the model in which the application
offer was created and `<offer name>` specifies the name of the application offer.


(offer-access-levels)=
## Offer access levels

(offer-access-levels-administrator)=
### `administrator`

Abilities: Can do anything that it is possible to do at the level of an offer.

(offer-access-levels-consumer)=
### `consumer`

Abilities: Can relate an application to the offer.

(offer-access-levels-reader)=
### `reader`

Abilities: Can view offers during a search with `juju find-offers`.