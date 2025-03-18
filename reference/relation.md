(relation)=
# Relation
> See also: {ref}`manage-relations`

In JAAS, following [Relation Based Access Control (**ReBAC**)](https://en.wikipedia.org/wiki/Relationship-based_access_control) and using [OpenFGA](https://openfga.dev/docs/concepts#what-is-a-relation), a relation is a tuple that represents the relationship between two entities A and B, where

- entity A ('object') is the entity that receives (or uses) the relationship and
- entity B ('target') is the entity that provides the relationship, and
- the relationship itself is about permission for A to perform an action on B; that is, a relation represents an entitlement of A on B.

For example, if a `user` named `alice@canonical.com` has the `member` relation with a group named `foo`, then the tuple that represents this relation will look like this:

```yaml
object:   user:alice@canonical.com
relation: member
target:   group:foo
```

This reads as: "an entity of type `user`, named `alice@canonical.com`, has the `member` relationship to an entity of type `group`, named `foo`.

> See more: {ref}`list-of-controller-relations`, {ref}`list-of-cloud-relations`, {ref}`list-of-model-relations`, {ref}`list-of-offer-relations`, {ref}`list-of-service-account-relations`, {ref}`list-of-role-relations`, {ref}`list-of-group-relations`



```{note}
**JAAS vs. OpenFGA.**
JAAS terminology is slightly different from OpenFGA. In OpenFGA, entity A and entity B are called 'user' and 'object', but in JAAS, they are called 'object' and 'target'.
```


```{important}
**Juju relations vs. JAAS relations:**

JAAS relations have nothing to the with {external+juju:ref}`Juju relations <relation>`, which refer to the relationship, or rather integration, of two charmed applications.

JAAS relations are currently similar to {external+juju:ref}`Juju access levels <user-access-levels>` -- they're both about permissions. The difference is that the relation paradigm is much broader; the list of relations is anticipated to become a superset of Juju access levels; and, while Juju access levels are always about a user's permissions on a controller, cloud, model, or offer, JAAS relations encompass many more entities on both sides, e.g., groups, roles, and service accounts.
```

