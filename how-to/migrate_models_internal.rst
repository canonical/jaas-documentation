JAAS: Internal Model Migration
==============================

This document briefly covers how to migrate a model between two controllers within JAAS.

JIMM can have multiple controllers connected to it, where more than 1 controller has access to the same cloud.
When a model is requested on such a cloud, JIMM will randomly select an appropriate controller.

The below is useful if you want to move the model to a specific controller.

Prerequisites
-------------

- A basic understanding of Juju model migrations, see the `docs <https://juju.is/docs/juju/manage-models#heading--migrate-a-workload-model-to-another-controller>`__.
- A running JAAS with with multiple controllers attached, see the :doc:`tutorial <../tutorial/deploy_jaas_microk8s>` for deploying JAAS.
- Administrator permissions for JAAS, so our :doc:`how-to <./bootstrap_permissions>`.

Connecting multiple controllers to JAAS can be accomplished adding LXD controllers as described in our :doc:`how-to <./add_controller>`.

1. Identify the new controller
------------------------------

JIMM does not currently expose information about which underlying controller hosts a specific model.
This information is stored in JIMM's database but the controller info returned when running ``juju show-model <model-name>``
is JIMM's UUID and name, hiding the underlying controller information. 

The following command will show you all the controllers connected to JIMM.

.. code:: bash

    jimmctl list-controllers

Currently to identify where the model is hosted, you must have access to the controllers connected to JIMM and switch to
those controllers in turn, and run ``juju models`` until you identify the correct controller. This may be improved in the future 
for users of ``jimmctl`` to identify the underlying controller for a model.

Identify the controller you want to migrate to, only the name is necessary.

2. Migrate your model
---------------------

The following command will migrate a model named ``my-model`` to the desired controller, in this case called ``prod-controller``.

.. code:: bash

    MODEL_NAME=my-model
    MODEL_UUID=$(juju show-model $MODEL_NAME --format yaml | yq .$MODEL_NAME.model-uuid)
    jimmctl migrate prod-controller model-$MODEL_UUID

This will start the model migration process. As a user of JIMM, you shouldn't notice that anything has changed. To confirm the
migration, switch to the controller originally hosting the model and verify that the model is no longer present.
