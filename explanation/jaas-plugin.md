(jaas-plugin)=
# `jaas` Plugin

The JAAS ecosystem introduces the JIMM controller. A special controller that sits in front of your Juju controllers
and acts as an authorisation gateway and aggregator.

Interacting with the JIMM controller is done in the same fashion as Juju controllers, i.e. using the Juju CLI.
JIMM also offers extra functionality, exposed by the `jaas` plugin.

This document explains how to install the `jaas` plugin and how it works.

## Installation

The `jaas` plugin is distributed as [a Snap](https://snapcraft.io/jaas).

```text
sudo snap install jaas --channel=3/stable
```

## How it works

The `jaas` CLI tool acts as a plugin for the Juju CLI.
When you install both the Juju and JAAS snaps, they automatically connect via Snap's
[content-interface](https://snapcraft.io/docs/content-interface) enabling new commands on the Juju CLI.

To view a list of all the newly available commands run `juju jaas -h`.

Plugin commands can either be executed with the `jaas` subcommand or directly, as follows:

```text
$ juju jaas <command>
or
$ juju <command>
```

The second form is preferred and used throughout our documentation.

These commands are intended to extend Juju's capabilities but note that some commands require elevated permissions.
