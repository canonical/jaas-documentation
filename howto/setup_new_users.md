# JAAS: Set up New Users

This how-to describes how a new user can begin to use JAAS to deploy applications.

## Prerequisites

For this how-to you will need the following:

- A running JAAS environment, see {doc}`our tutorial <../tutorial/deploy_jaas_microk8s>`.
- A controller connected to JAAS, see {doc}`our how-to <./add_controller>`. This how-to assumes you have added an LXD controller.

## Login to JAAS

Login to JAAS for the first time:

```text
juju login test-jimm.localhost:443 -c jaas
```

Running `juju controllers` will now show you the `jaas` controller.
Commands like `juju models` will now work.

## Cloud Access

Verify that you have access to a cloud.

You can check this by running:

```text
juju clouds --controller jimm
```

The response should resemble:

```text
Clouds available on the controller:
Cloud      Regions  Default    Type
localhost  1        localhost  lxd
```

If no clouds are available, an administrator may need to grant you
`add-model` permissions on this cloud. See the [`juju grant-cloud`](https://canonical-juju.readthedocs-hosted.com/en/latest/user/reference/juju-cli/list-of-juju-cli-commands/grant-cloud/) command for details.

## Cloud Credentials

Before we can create a model, your user requires a set of cloud [credentials](https://juju.is/docs/juju/credential).

View credentials available on your client:

```text
juju credentials --client
```

The output should resemble the following

```text
Client Credentials:
Cloud      Credentials
localhost  localhost*
microk8s   microk8s*
```

We will upload credentials for LXD - the `localhost` credentials.

```text
juju update-credentials localhost --controller jimm
```

```{tip}
For clouds like Openstack or public clouds, use the `juju add-credential` command to interactively add credentials to your client and controller.
```

Credentials are now available on the controller for your user to deploy models on the LXD cloud.

## Deploy

Now we can deploy a model and application:

```text
juju add-model test-model
juju deploy ubuntu
```

We've successfully added a set of cloud-credentials and deployed an application into a model.