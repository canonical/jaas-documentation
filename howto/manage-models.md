(manage-models)=
# Manage models

(migrate-a-model-to-jimm)=
## Migrate a model to JIMM

In many scenarios it is necessary to migrate models to and from an environment that includes JAAS.

### Prerequisites

- A standalone Juju controller with a model (optionally with a running application).
- A basic understanding of Juju model migrations, see the [docs](https://juju.is/docs/juju/manage-models).
- A running JAAS, see the {doc}`the tutorial <../tutorial/index>`.
- Administrator permissions for JAAS, see our {doc}`how-to <./manage-your-jaas-deployment>`.

### 1. Create a new Juju controller

This is only necessary if you have a Juju controller that does not have the `login-token-refresh-url` config option set to point
at a running JIMM instance. Use the following command to check if your controller is configured.

```text
juju switch <controller-name>
juju controller-config login-token-refresh-url
```

If the value is empty, the bootstrapping of a new controller with this configuration value
is required.
In order to use models with JAAS, the models must be running on a Juju controller that is properly configured. The
necessary config values cannot be set after bootstrap time, so any existing models must be migrated to a new controller.

The process of creating a local Juju controller that is properly configured is described in {ref}`add-a-juju-controller`.

Once a Juju controller that is configured to communicate with JIMM has been created, move onto the next step.

### 2. Migrate desired models

Once you have identified which models to migrate, we will begin the process of model migration.

We will assume a model called `my-model` is currently hosted on a controller called `my-controller` and moving to a new controller
called `workload-lxd` (`workload-lxd` should be connected to JIMM).

```text
juju switch my-controller:my-model
juju migrate my-model workload-lxd
juju status --watch 2s
# Wait for model migration to complete.
juju switch workload-lxd
juju models
```

At this point we should see the model has been migrated.

### 3. Import the model into JIMM

Finally we will import the model into JIMM.

First we must check that we have a cloud-credential for the cloud where the desired model is running.
This is simply a pre-check performed when importing a model to ensure that the user has credentials for the cloud.

Check with the following,

```text
juju switch jimm
juju list-credentials --controller
```

If you do not see a cloud-credential for the desired cloud, you can add one by following the instructions in [managing cloud-credentials](https://juju.is/docs/juju/manage-credentials).

We then need the model UUID to import the model.

```text
MODEL_NAME="my-model"
juju switch workload-lxd:$MODEL_NAME
MODEL_UUID=$(juju show-model $MODEL_NAME --format yaml | yq .$MODEL_NAME.model-uuid)
juju switch jimm-k8s
# Replace <user-email> below with your email address
juju import-model workload-lxd $MODEL_UUID --owner <username>
juju models
# The new model should now be visible
```

With that the model should now be visible in JIMM. The purpose of the `--owner` flag is to tell JIMM who
the new model owner should be. Models created on Juju controllers use local users while JIMM requires external
identities for all users.

At this point you can grant other users access to the model. See Juju documentation for [more info](https://juju.is/docs/juju/user-permissions).

Migrating the model back to the original controller is also possible using the same migrate command as used in step 2.
Switch to the `workload-lxd` controller where the model now lives and run the same steps to migrate back to `my-controller`.

(migrate-a-model-within-jimm)=
## Migrate a model within JIMM

This document briefly covers how to migrate a model between two controllers within JAAS.

JIMM can have multiple controllers connected to it, where more than 1 controller has access to the same cloud.
When a model is requested on such a cloud, JIMM will randomly select an appropriate controller.

The below is useful if you want to move the model to a specific controller.

### Prerequisites

- A basic understanding of Juju model migrations, see the [docs](https://juju.is/docs/juju/manage-models).
- A running JAAS with with multiple controllers attached, see the {doc}`the tutorial <../tutorial/index>` for deploying JAAS.
- Administrator permissions for JAAS, so our {ref}`add-a-juju-controller`.

Connecting multiple controllers to JAAS can be accomplished adding LXD controllers as described in {ref}`add-a-juju-controller`.

### 1. Identify the new controller

JIMM does not currently expose information about which underlying controller hosts a specific model.
This information is stored in JIMM's database but the controller info returned when running `juju show-model <model-name>`
is JIMM's UUID and name, hiding the underlying controller information.

The following command will show you all the controllers connected to JIMM.

```text
juju list-controllers --managed
```

Currently to identify where the model is hosted, you must have access to the controllers connected to JIMM and switch to
those controllers in turn, and run `juju models` until you identify the correct controller.

Identify the controller you want to migrate to, only the name is necessary.

### 2. Migrate your model

The following command will migrate a model named `my-model` to the desired controller, in this case called `my-controller`.

```text
MODEL_NAME=my-model
MODEL_UUID=$(juju show-model $MODEL_NAME --format yaml | yq .$MODEL_NAME.model-uuid)
juju jaas migrate my-controller $MODEL_UUID
```

This will start the model migration process. You can now monitor the progress of the migration with `juju status` and `juju debug-log`.

Once the model has been successfully migrated, run the following command to update JIMM with the new controller information for the model.

```text
juju update-migrated-model my-controller $MODEL_UUID
```

This will update JIMM's internal state to locate the model on the specified controller.

At this point you can run `juju status` to see the model info.

### 3. Handling failed migration

If the model migration fails, then no further user input is required and the model should continue to exist on the original controller.

To inspect the reason for failure, consult the output from `juju debug-log` and `juju status`.

(manage-an-entitys-relation-to-a-model)=
## Manage an entity's relation to a model

See {ref}`manage-permissions`.
