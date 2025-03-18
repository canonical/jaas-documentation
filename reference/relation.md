(relation)=
# Relation
> See also: {ref}`manage-relations`

In JAAS, like in [OpenFGA](https://openfga.dev/docs/concepts#what-is-a-relation), a relation is about Relation Based Access Control (**ReBAC**) and describes the relationship between two entities.


```{important}
**Juju relations vs. JAAS relations:**

JAAS relations have nothing to the with {external+juju:ref}`Juju relations <relation>`, which refer to the relationship, or rather integration, of two charmed applications.

JAAS relations are currently similar to {external+juju:ref}`Juju access levels <user-access-levels>` -- they're both about permissions. The difference is that the relation paradigm is much broader; the list of relations is anticipated to become a superset of Juju access levels; and, while Juju access levels are always about a user's permissions on a controller, cloud, model, or offer, JAAS relations encompass many more entities on both sides, e.g., groups, roles, and service accounts.
```

> See more: {ref}`list-of-controller-relations`, {ref}`list-of-cloud-relations`, {ref}`list-of-model-relations`, {ref}`list-of-offer-relations`, {ref}`list-of-service-account-relations`, {ref}`list-of-role-relations`, {ref}`list-of-group-relations`
