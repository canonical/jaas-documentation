JAAS: Model Migration
=====================

In many scenarios it is necessary to migrate models from existing Juju controllers
to an environment that includes JAAS.

Prerequisites
-------------

- A standalone Juju controller with a model (optionally with a running application).
- A basic understanding of Juju model migrations, see the `docs <https://juju.is/docs/juju/manage-models>`__.
- A running JAAS, see the :doc:`tutorial <../tutorial/deploy_jaas_microk8s>`.
- Administrator permissions for JAAS, so our :doc:`how-to <./bootstrap_permissions>`.

1. Create a new Juju controller
-------------------------------

This is only necessary if you have a Juju controller that does not have the ``login-token-refresh-url`` config option set to point 
at a running JIMM instance. Use the following command to check if your controller is configured.

.. code:: bash

    juju switch <controller-name>
    juju controller-config login-token-refresh-url

An empty value indicates that a new controller is necessary.

In order to use models with JAAS, the models must be running on a Juju controller that is properly configured. The
necessary config values cannot be set after bootstrap time, so any existing models must be migrated to a new controller.

The process of creating a local Juju controller that is properly configured is described in :doc:`this how-to <./add_controller>`.
 
Once a Juju controller that is configured to communicate with JIMM has been created, move onto the next step.

2. Migrate desired models
-------------------------

Once you have identified which models to migrate, we will begin the process of model migration.

We will assume a model called ``my-model`` is currently hosted on a controller called ``my-controller`` and moving to a new controller 
called ``workload-lxd`` (``workload-lxd`` should be connected to JIMM).

.. code:: bash

    juju switch my-controller:my-model
    juju migrate my-model workload-lxd
    juju status --watch 2s
    # Wait for model migration to complete.
    juju switch workload-lxd
    juju models

At this point we should see the model has been migrated.

3. Import the model into JIMM
-----------------------------

Finally we will import the model into JIMM using ``jimmctl``.

First we must check that we have a cloud-credential for the cloud where the desired model is running.
This is simply a pre-check performed when importing a model to ensure that the user has credentials for the cloud.

Check with the following,

.. code:: bash

    juju switch jimm
    juju list-credentials --controller

If you do not see a cloud-credential for the desired cloud, you can add one by following the instructs on
`managing cloud-credentials <https://juju.is/docs/juju/manage-credentials>`__. 

We then need the model UUID to import the model.

.. code:: bash

    MODEL_NAME="my-model"
    juju switch workload-lxd:$MODEL_NAME
    MODEL_UUID=$(juju show-model $MODEL_NAME --format yaml | yq .$MODEL_NAME.model-uuid)
    juju switch jimm-k8s
    # Replace <user-email> below with your email address
    jimmctl import-model workload-lxd $MODEL_UUID --owner <username>
    juju models
    # The new model should now be visible

With that the model should now be visible in JIMM. The purpose of the ``--owner`` flag is to tell JIMM who 
the new model owner should be. Models created on Juju controllers use local users while JIMM requires external
identities for all users.

At this point you can grant other users access to the model.


