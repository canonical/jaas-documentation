(model)=
# Model
> See also: {ref}`model`

(model-tag)=
## Model tag
> See first: {ref}`tag`

A model tag has the following format:

```text
model-<controller name>/<model name>
```

where `<controller name>` specifies name of the controller on which the model
is running and `<model name>` specifies the name of the model.

(model-access-levels)=
## Model access levels

(model-access-levels-reader)=
### `reader`

Abilities: Can view the content of a model without changing it. Can use any of the read commands.

(model-access-levels-writer)=
### `writer`

Abilities: Can deploy and manage applications on the model.

(model-access-levels-administrator)=
### `administrator`

Abilities: Can do anything that it is possible to do at the level of a model.This grants permissions to all resources that inherit from model access.