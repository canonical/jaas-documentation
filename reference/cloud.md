(cloud)=
# Cloud
> See first: {external+juju:ref}`Juju | Cloud <cloud>`


(cloud-tag)=
## Cloud tag
> See first: {ref}`tag`

A cloud tag has the following format:

```text
cloud-<cloud name>
```

(cloud-relation)=
## Cloud relation

A cloud relation is a {ref}`relation <relation>` that describes permissions on a cloud.

(list-of-cloud-relations)=
### List of cloud relations

(cloud-relation-administrator)=
#### `administrator`

Abilities: Can do anything that it is possible to do at the level of a cloud.

(cloud-relation-can-addmodel)=
#### `can_addmodel`

Abilities: Can add a model and grant another user model-level permissions.