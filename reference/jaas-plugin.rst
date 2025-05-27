``jaas`` plugin
###############

jaas add-cloud
==============

Summary
-------

Add cloud to specific controller in jimm

Usage
-----

``jaas add-cloud [options] <controller_name> <cloud_name>``

Options
~~~~~~~

+--------------------------------+-----------------------+------------------------------------------------+
| Flag                           | Default               | Usage                                          |
+================================+=======================+================================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication      |
+--------------------------------+-----------------------+------------------------------------------------+
| ``--cloud``                    |                       | The path to the cloud’s definition file.       |
+--------------------------------+-----------------------+------------------------------------------------+
| ``--force``                    | false                 | Forces the cloud to be added to the controller |
+--------------------------------+-----------------------+------------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)              |
+--------------------------------+-----------------------+------------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                         |
+--------------------------------+-----------------------+------------------------------------------------+

Examples
--------

::

   juju add-cloud mycontroller mycloud
   juju add-cloud mycontroller mycloud --cloud=./cloud-definition.yaml

Details
-------

Adds the specified cloud to a specific controller on JIMM.

One can specify a cloud definition via a yaml file passed with the –cloud flag. If the flag is missing, the command will assume the cloud definition is already known and will error otherwise.

jaas add-group
==============

.. _summary-1:

Summary
-------

Add group to jimm.

.. _usage-1:

Usage
-----

``jaas add-group [options] <name>``

.. _options-1:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-1:

Examples
--------

::

   juju add-group

.. _details-1:

Details
-------

Adds a group.

jaas add-permission
===================

.. _summary-2:

Summary
-------

Add relation to JIMM.

.. _usage-2:

Usage
-----

``jaas add-permission [options] <object> <relation> <target_object>``

.. _options-2:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-f``                         |                       | file location of JSON encoded tuples      |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-2:

Examples
--------

::

   juju add-permission user-alice@canonical.com member group-mygroup
   juju add-permission group-MyTeam#member admin model-mymodel
   juju add-permission -f /path/to/file.yaml

.. _details-2:

Details
-------

Grants access to a resource.

This command works at a low-level and commands like ‘juju grant’ should be preferred in most cases.

Permissions in JIMM consist of an object, a relation and a target object. These are used to define access control between resources.

The object and target object must be of the form <tag>-<objectname> or <tag>-<object-uuid> E.g. “user-Alice” or “controller-MyController”

-f Read from a file where filename is the location of a JSON encoded file of the form: [ { “object”:“user-mike”, “relation”:“member”, “target_object”:“group-yellow” }, { “object”:“user-alice”, “relation”:“member”, “target_object”:“group-yellow” } ]

Certain constraints apply when creating/removing permissions, namely: Object may be one of:

::

   user tag                = "user-<name>"
   group tag               = "group-<name>"
   controller tag          = "controller-<name>"
   model tag               = "model-<name>"
   application offer tag   = "offer-<name>"

If target_object is a group, the relation can only be:

::

   member

If target_object is a controller, the relation can be one of:

::

   loginer
   administrator

If target_object is a model, the relation can be one of:

::

   reader
   writer
   administrator

If target_object is an application offer, the relation can be one of:

::

   reader
   consumer
   administrator

Additionally, if the object is a group, a userset can be applied by adding #member as follows. This will grant/revoke access to all users within TeamA:

::

   group-TeamA#member administrator controller-MyController

jaas add-role
=============

.. _summary-3:

Summary
-------

Add role to jimm.

.. _usage-3:

Usage
-----

``jaas add-role [options] <role name>``

.. _options-3:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-3:

Examples
--------

::

   juju add-role myrole 

.. _details-3:

Details
-------

Adds a role.

jaas add-service-account
========================

.. _summary-4:

Summary
-------

Add permission to manage a service account

.. _usage-4:

Usage
-----

``jaas add-service-account [options] <client-id>``

.. _options-4:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-4:

Examples
--------

::

   juju add-service-account <client-id> 

.. _details-4:

Details
-------

add-service-account binds a service account to your user, giving you administrator access over the service account. Can only be run once per service account.

jaas audit-events
=================

**Aliases:** audit-events

.. _summary-5:

Summary
-------

Displays audit events

.. _usage-5:

Usage
-----

``jaas list-audit-events [options]``

.. _options-5:

Options
~~~~~~~

+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| Flag                           | Default               | Usage                                                                     |
+================================+=======================+===========================================================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication                                 |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--after``                    |                       | display events that happened after a specified time, formatted as RFC3339 |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--before``                   |                       | display events that happened before specified time, formatted as RFC3339  |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|tabular|yaml)                                 |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--limit``                    | 0                     | limit the maximum number of returned audit events                         |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--method``                   |                       | display events for a specific method call                                 |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--model``                    |                       | display events for a specific model (model name is controller/model)      |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                                                    |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--offset``                   | 0                     | offset the set of returned audit events                                   |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--reverse``                  | false                 | reverse the order of logs, showing the most recent first                  |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--user-tag``                 |                       | display events performed by authenticated user                            |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+

.. _examples-5:

Examples
--------

::

   juju list-audit-events --after 2020-01-01T15:00:00 --before 2020-01-01T15:00:00 --user-tag user@canonical.com --limit 50
   juju list-audit-events --method CreateModel
   juju audit-events --after 2020-01-01T15:00:00 --format yaml

.. _details-5:

Details
-------

Returns audit log events.

jaas check-permission
=====================

.. _summary-6:

Summary
-------

Check access to a resource.

.. _usage-6:

Usage
-----

``jaas check-permission [options] <object> <relation> <target_object>``

.. _options-6:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | smart                 | Specify output format (json|smart|yaml)   |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-6:

Examples
--------

::

   juju check-permission user-alice@canonical.com administrator controller-aws-controller-1

.. _details-6:

Details
-------

Verifies access to a resource.

jaas controllers
================

**Aliases:** list-controllers

.. _summary-7:

Summary
-------

Lists all controllers known to JIMM.

.. _usage-7:

Usage
-----

``jaas controllers [options]``

.. _options-7:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-7:

Examples
--------

::

   juju controllers 
   juju controllers --format json

.. _details-7:

Details
-------

Displays controller information for all controllers known to JIMM.

jaas documentation
==================

.. _summary-8:

Summary
-------

Generate the documentation for all commands

.. _usage-8:

Usage
-----

``jaas documentation [options] --out <target-folder> --no-index --split --url <base-url> --discourse-ids <filepath>``

.. _options-8:

Options
~~~~~~~

+-----------------------+-----------------------+------------------------------------------------------------------------------------------+
| Flag                  | Default               | Usage                                                                                    |
+=======================+=======================+==========================================================================================+
| ``--discourse-ids``   |                       | File containing a mapping of commands and their discourse ids                            |
+-----------------------+-----------------------+------------------------------------------------------------------------------------------+
| ``--no-index``        | false                 | Do not generate the commands index                                                       |
+-----------------------+-----------------------+------------------------------------------------------------------------------------------+
| ``--out``             |                       | Documentation output folder if not set the result is displayed using the standard output |
+-----------------------+-----------------------+------------------------------------------------------------------------------------------+
| ``--split``           | false                 | Generate a separate Markdown file for each command                                       |
+-----------------------+-----------------------+------------------------------------------------------------------------------------------+
| ``--url``             |                       | Documentation host URL                                                                   |
+-----------------------+-----------------------+------------------------------------------------------------------------------------------+

.. _examples-8:

Examples
--------

::

   juju documentation
   juju documentation --split 
   juju documentation --split --no-index --out /tmp/docs

To render markdown documentation using a list of existing commands, you can use a file with the following syntax

::

   command1: id1
   command2: id2
   commandN: idN

For example:

::

   add-cloud: 1183
   add-secret: 1284
   remove-cloud: 4344

Then, the urls will be populated using the ids indicated in the file above.

::

   juju documentation --split --no-index --out /tmp/docs --discourse-ids /tmp/docs/myids

.. _details-8:

Details
-------

This command generates a markdown formatted document with all the commands, their descriptions, arguments, and examples.

jaas grant-audit-log
====================

.. _summary-9:

Summary
-------

Grants access to audit logs.

.. _usage-9:

Usage
-----

``jaas grant-audit-log [options] <username>``

.. _options-9:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-9:

Examples
--------

::

   juju grant-audit-log <username> 

.. _details-9:

Details
-------

Grants a user access to read audit logs.

jaas grant-service-account-access
=================================

.. _summary-10:

Summary
-------

Grants administrator access over a service account

.. _usage-10:

Usage
-----

``jaas grant-service-account-access [options] <client-id> (<user>|<group>) [(<user>|<group>) ...]``

.. _options-10:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | smart                 | Specify output format (smart)             |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-10:

Examples
--------

::

   juju grant-service-account-access 00000000-0000-0000-0000-000000000000 user-foo group-bar

.. _details-10:

Details
-------

grant-service-account-access grants administrator access over a service account to the given groups/identities.

jaas groups
===========

**Aliases:** groups

.. _summary-11:

Summary
-------

List all groups.

.. _usage-11:

Usage
-----

``jaas list-groups [options]``

.. _options-11:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--limit``                    | 0                     | The maximum number of groups to return    |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--offset``                   | 0                     | The offset to use when requesting groups  |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-11:

Examples
--------

::

   juju list-groups

.. _details-11:

Details
-------

Lists all groups.

jaas help
=========

.. _summary-12:

Summary
-------

Show help on a command or other topic.

.. _usage-12:

Usage
-----

``jaas help [flags] [topic]``

.. _details-12:

Details
-------

See also: topics

jaas import-model
=================

**Aliases:** register-model

.. _summary-13:

Summary
-------

Import a model to jimm

.. _usage-13:

Usage
-----

``jaas import-model [options] <controller name> <model uuid>``

.. _options-12:

Options
~~~~~~~

+--------------------------------+-----------------------+--------------------------------------------+
| Flag                           | Default               | Usage                                      |
+================================+=======================+============================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication  |
+--------------------------------+-----------------------+--------------------------------------------+
| ``--owner``                    |                       | switch the model owner to the desired user |
+--------------------------------+-----------------------+--------------------------------------------+

.. _examples-12:

Examples
--------

::

   juju import-model mycontroller ac30d6ae-0bed-4398-bba7-75d49e39f189
   juju import-model mycontroller ac30d6ae-0bed-4398-bba7-75d49e39f189 --owner user@canonical.com

.. _details-13:

Details
-------

Imports a model running on a controller into JIMM’s state.

When importing, it is necessary for JIMM to contain a set of cloud credentials that represent a user’s access to the incoming model’s cloud.

The –owner command is necessary when importing a model created by a local user and it will switch the model owner to the desired external user.

jaas list-audit-events
======================

**Aliases:** audit-events

.. _summary-14:

Summary
-------

Displays audit events

.. _usage-14:

Usage
-----

``jaas list-audit-events [options]``

.. _options-13:

Options
~~~~~~~

+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| Flag                           | Default               | Usage                                                                     |
+================================+=======================+===========================================================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication                                 |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--after``                    |                       | display events that happened after a specified time, formatted as RFC3339 |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--before``                   |                       | display events that happened before specified time, formatted as RFC3339  |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|tabular|yaml)                                 |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--limit``                    | 0                     | limit the maximum number of returned audit events                         |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--method``                   |                       | display events for a specific method call                                 |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--model``                    |                       | display events for a specific model (model name is controller/model)      |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                                                    |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--offset``                   | 0                     | offset the set of returned audit events                                   |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--reverse``                  | false                 | reverse the order of logs, showing the most recent first                  |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+
| ``--user-tag``                 |                       | display events performed by authenticated user                            |
+--------------------------------+-----------------------+---------------------------------------------------------------------------+

.. _examples-13:

Examples
--------

::

   juju list-audit-events --after 2020-01-01T15:00:00 --before 2020-01-01T15:00:00 --user-tag user@canonical.com --limit 50
   juju list-audit-events --method CreateModel
   juju audit-events --after 2020-01-01T15:00:00 --format yaml

.. _details-14:

Details
-------

Returns audit log events.

jaas list-controllers
=====================

**Aliases:** list-controllers

.. _summary-15:

Summary
-------

Lists all controllers known to JIMM.

.. _usage-15:

Usage
-----

``jaas controllers [options]``

.. _options-14:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-14:

Examples
--------

::

   juju controllers 
   juju controllers --format json

.. _details-15:

Details
-------

Displays controller information for all controllers known to JIMM.

jaas list-groups
================

**Aliases:** groups

.. _summary-16:

Summary
-------

List all groups.

.. _usage-16:

Usage
-----

``jaas list-groups [options]``

.. _options-15:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--limit``                    | 0                     | The maximum number of groups to return    |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--offset``                   | 0                     | The offset to use when requesting groups  |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-15:

Examples
--------

::

   juju list-groups

.. _details-16:

Details
-------

Lists all groups.

jaas list-permissions
=====================

**Aliases:** permissions

.. _summary-17:

Summary
-------

List relations.

.. _usage-17:

Usage
-----

``jaas list-permissions [options]``

.. _options-16:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|tabular|yaml) |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--object``                   |                       | relation object                           |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--relation``                 |                       | relation name                             |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--resolve``                  | true                  | resolves UUIDs to human readable tags     |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--target``                   |                       | relation target object                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-16:

Examples
--------

List all permissions

::

   juju list-permissions

List permissions where the target object match

::

   juju list-permissions --target model-mymodel

List permissions where the target object and relation match

::

   juju list-permissions --target model-mymodel  --relation admin

.. _details-17:

Details
-------

List permissions known to JIMM. Using the “target”, “relation” and “object” flags, only those permissions matching the filter will be returned.

jaas list-roles
===============

**Aliases:** roles

.. _summary-18:

Summary
-------

List all roles.

.. _usage-18:

Usage
-----

``jaas list-roles [options]``

.. _options-17:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--limit``                    | 0                     | The maximum number of roles to return     |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--offset``                   | 0                     | The offset to use when requesting roles   |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-17:

Examples
--------

::

   juju list-roles list

.. _details-18:

Details
-------

Lists all roles.

jaas list-service-account-credentials
=====================================

.. _summary-19:

Summary
-------

List service account cloud credentials

.. _usage-19:

Usage
-----

``jaas list-service-account-credentials [options] <client-id>``

.. _options-18:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------------------+
| Flag                           | Default               | Usage                                                 |
+================================+=======================+=======================================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication             |
+--------------------------------+-----------------------+-------------------------------------------------------+
| ``--format``                   | tabular               | Specify output format (json|tabular|yaml)             |
+--------------------------------+-----------------------+-------------------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                                |
+--------------------------------+-----------------------+-------------------------------------------------------+
| ``--show-secrets``             | false                 | Show secrets, applicable to yaml or json formats only |
+--------------------------------+-----------------------+-------------------------------------------------------+

.. _examples-18:

Examples
--------

::

   juju list-service-account-credentials <client-id> 
   juju list-service-account-credentials <client-id> --show-secrets
   juju list-service-account-credentials <client-id> --format yaml

.. _details-19:

Details
-------

list-service-account-credentials lists the cloud credentials belonging to a service account.

This command only shows credentials uploaded to the controller that belong to the service account. Client-side credentials should be managed via the juju credentials command.

jaas migrate
============

.. _summary-20:

Summary
-------

Migrate models to the target controller

.. _usage-20:

Usage
-----

``jaas migrate [options] <controller name> <model uuid> [<model uuid>...]``

.. _options-19:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-19:

Examples
--------

::

   juju migrate mycontroller 2cb433a6-04eb-4ec4-9567-90426d20a004 fd469983-27c2-423b-bebf-84f616fb036b ...
   juju migrate mycontroller user@domain.com/model-a user@domain.com/model-b ...
   juju migrate mycontroller user@domain.com/model-a fd469983-27c2-423b-bebf-84f616fb036b ...

.. _details-20:

Details
-------

The migrate commands migrates a model, or many models between two controllers registered within JIMM.

You may specify a model name (of the form owner/name) or model UUID.

jaas model-status
=================

.. _summary-21:

Summary
-------

Displays full model status

.. _usage-21:

Usage
-----

``jaas model-status [options] <model uuid>``

.. _options-20:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-20:

Examples
--------

::

   juju model-status 2cb433a6-04eb-4ec4-9567-90426d20a004 
   juju model-status 2cb433a6-04eb-4ec4-9567-90426d20a004 --format yaml

.. _details-21:

Details
-------

Displays full model status.

jaas permissions
================

**Aliases:** permissions

.. _summary-22:

Summary
-------

List relations.

.. _usage-22:

Usage
-----

``jaas list-permissions [options]``

.. _options-21:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|tabular|yaml) |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--object``                   |                       | relation object                           |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--relation``                 |                       | relation name                             |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--resolve``                  | true                  | resolves UUIDs to human readable tags     |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--target``                   |                       | relation target object                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-21:

Examples
--------

List all permissions

::

   juju list-permissions

List permissions where the target object match

::

   juju list-permissions --target model-mymodel

List permissions where the target object and relation match

::

   juju list-permissions --target model-mymodel  --relation admin

.. _details-22:

Details
-------

List permissions known to JIMM. Using the “target”, “relation” and “object” flags, only those permissions matching the filter will be returned.

jaas purge-audit-logs
=====================

.. _summary-23:

Summary
-------

purge audit logs from the database before the given date

.. _usage-23:

Usage
-----

``jaas purge-audit-logs [options] <date>``

.. _options-22:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-22:

Examples
--------

::

   juju purge-audit-logs 2021-02-03
   juju purge-audit-logs 2021-02-03T00
   juju purge-audit-logs 2021-02-03T15:04:05Z  

.. _details-23:

Details
-------

Purges logs from the database before the given date.

The provided date must be formatted as an ISO8601 date string.

jaas query-models
=================

.. _summary-24:

Summary
-------

Query model statuses

.. _usage-24:

Usage
-----

``jaas query-models [options] <query>``

.. _options-23:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | json                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-23:

Examples
--------

::

   juju query-models '.applications | with_entries(select(.key=="nginx-ingress-integrator"))'

.. _details-24:

Details
-------

Queries all models available to the current user performing the query against each model status individually, returning the collated query responses for each model.

The query runs against the output of “juju status –format json”, as such you can format your query against an output like this.

The queries expect a JQ query string.

jaas register-controller
========================

.. _summary-25:

Summary
-------

Add controller to jimm

.. _usage-25:

Usage
-----

``jaas register-controller [options] <filepath>``

.. _options-24:

Options
~~~~~~~

+--------------------------------+-----------------------+------------------------------------------------------------------------------------------------------+
| Flag                           | Default               | Usage                                                                                                |
+================================+=======================+======================================================================================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication                                                            |
+--------------------------------+-----------------------+------------------------------------------------------------------------------------------------------+
| ``--dry-run``                  | false                 | Dry-run enabled will only print the controller details.                                              |
+--------------------------------+-----------------------+------------------------------------------------------------------------------------------------------+
| ``--file``                     |                       | Specify a file-path for controller details, use ‘-’ to read from stdin.                              |
+--------------------------------+-----------------------+------------------------------------------------------------------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)                                                                    |
+--------------------------------+-----------------------+------------------------------------------------------------------------------------------------------+
| ``--local``                    | false                 | If local flag is specified, then the local API addresses and CA cert of the controller will be used. |
+--------------------------------+-----------------------+------------------------------------------------------------------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                                                                               |
+--------------------------------+-----------------------+------------------------------------------------------------------------------------------------------+
| ``--public-address``           |                       | Specify a custom public address to use for dialing the controller.                                   |
+--------------------------------+-----------------------+------------------------------------------------------------------------------------------------------+
| ``--tls-hostname``             |                       | Specify the hostname for TLS verification.                                                           |
+--------------------------------+-----------------------+------------------------------------------------------------------------------------------------------+

.. _examples-24:

Examples
--------

::

   juju register-controller mycontroller
   juju register-controller mycontroller --local

.. _details-25:

Details
-------

Registers a controller with JIMM.

Using the controller name provided, this command will inspect your Juju client store for details on the specified controller.

Note that by default, this command assumes the controller has the public-hostname field set, which will define the preferred address JIMM will use to contact the controller. Use of a public address will also ignore any custom CA cert in your local client store and assumes the server is secured with a public certificate.

Use the –local flag if the server is not configured with a public address or to ignore the controller’s public-hostname and use the custom CA of the controller.

A yaml formatted file can also be used as input for cases where the controller is not available on the client. Using the –file will validate that the provided controller name matches the name in the yaml file. Using –file will ignore other flags like –public-address and –local.

Use the –dry-run flag to generate a sample file without registering the controller. This can be used later as input to register-controller.

jaas register-model
===================

**Aliases:** register-model

.. _summary-26:

Summary
-------

Import a model to jimm

.. _usage-26:

Usage
-----

``jaas import-model [options] <controller name> <model uuid>``

.. _options-25:

Options
~~~~~~~

+--------------------------------+-----------------------+--------------------------------------------+
| Flag                           | Default               | Usage                                      |
+================================+=======================+============================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication  |
+--------------------------------+-----------------------+--------------------------------------------+
| ``--owner``                    |                       | switch the model owner to the desired user |
+--------------------------------+-----------------------+--------------------------------------------+

.. _examples-25:

Examples
--------

::

   juju import-model mycontroller ac30d6ae-0bed-4398-bba7-75d49e39f189
   juju import-model mycontroller ac30d6ae-0bed-4398-bba7-75d49e39f189 --owner user@canonical.com

.. _details-26:

Details
-------

Imports a model running on a controller into JIMM’s state.

When importing, it is necessary for JIMM to contain a set of cloud credentials that represent a user’s access to the incoming model’s cloud.

The –owner command is necessary when importing a model created by a local user and it will switch the model owner to the desired external user.

jaas remove-cloud
=================

.. _summary-27:

Summary
-------

Remove cloud from specific controller in jimm

.. _usage-27:

Usage
-----

``jaas remove-cloud [options] <controller_name> <cloud_name>``

.. _options-26:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-26:

Examples
--------

::

   juju remove-cloud mycontroller mycloud

.. _details-27:

Details
-------

Removes the specified cloud from the specified controller in JIMM.

jaas remove-group
=================

.. _summary-28:

Summary
-------

Remove a group.

.. _usage-28:

Usage
-----

``jaas remove-group [options] <name>``

.. _options-27:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | smart                 | Specify output format (smart)             |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-y``                         | false                 | delete group without prompt               |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-27:

Examples
--------

::

   juju remove-group mygroup

.. _details-28:

Details
-------

Removes a group.

jaas remove-permission
======================

.. _summary-29:

Summary
-------

Remove relation from JIMM.

.. _usage-29:

Usage
-----

``jaas remove-permission [options] <object> <relation> <target_object>``

.. _options-28:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-f``                         |                       | file location of JSON encoded tuples      |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-28:

Examples
--------

::

   juju remove-permission user-alice@canonical.com member group-mygroup
   juju remove-permission group-MyTeam#member admin model-mymodel
   juju remove-permission -f /path/to/file.yaml

.. _details-29:

Details
-------

Revokes access to a resource.

This command works at a low-level and commands like ‘juju grant’ should be preferred in most cases.

Permissions in JIMM consist of an object, a relation and a target object. These are used to define access control between resources.

The object and target object must be of the form <tag>-<objectname> or <tag>-<object-uuid> E.g. “user-Alice” or “controller-MyController”

-f Read from a file where filename is the location of a JSON encoded file of the form: [ { “object”:“user-mike”, “relation”:“member”, “target_object”:“group-yellow” }, { “object”:“user-alice”, “relation”:“member”, “target_object”:“group-yellow” } ]

Certain constraints apply when creating/removing permissions, namely: Object may be one of:

::

   user tag                = "user-<name>"
   group tag               = "group-<name>"
   controller tag          = "controller-<name>"
   model tag               = "model-<name>"
   application offer tag   = "offer-<name>"

If target_object is a group, the relation can only be:

::

   member

If target_object is a controller, the relation can be one of:

::

   loginer
   administrator

If target_object is a model, the relation can be one of:

::

   reader
   writer
   administrator

If target_object is an application offer, the relation can be one of:

::

   reader
   consumer
   administrator

Additionally, if the object is a group, a userset can be applied by adding #member as follows. This will grant/revoke access to all users within TeamA:

::

   group-TeamA#member administrator controller-MyController

jaas remove-role
================

.. _summary-30:

Summary
-------

Remove a role.

.. _usage-30:

Usage
-----

``jaas remove-role [options] <role name>``

.. _options-29:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | smart                 | Specify output format (smart)             |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-y``                         | false                 | delete role without prompt                |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-29:

Examples
--------

::

   juju remove-role remove myrole

.. _details-30:

Details
-------

Removes a role.

jaas rename-group
=================

.. _summary-31:

Summary
-------

Rename a group.

.. _usage-31:

Usage
-----

``jaas rename-group [options] <name> <new name>``

.. _options-30:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-30:

Examples
--------

::

   juju rename-group mygroup newgroup

.. _details-31:

Details
-------

Renames a group.

jaas rename-role
================

.. _summary-32:

Summary
-------

Rename a role.

.. _usage-32:

Usage
-----

``jaas rename-role [options] <role name> <new role name>``

.. _options-31:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-31:

Examples
--------

::

   juju rename-role myrole newrolename

.. _details-32:

Details
-------

Renames a role.

jaas revoke-audit-log
=====================

.. _summary-33:

Summary
-------

revokes access to audit logs.

.. _usage-33:

Usage
-----

``jaas revoke-audit-log [options] <user>``

.. _options-32:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-32:

Examples
--------

::

   juju revoke-audit-log user@canonical.com

.. _details-33:

Details
-------

Revokes user access to audit logs.

jaas roles
==========

**Aliases:** roles

.. _summary-34:

Summary
-------

List all roles.

.. _usage-34:

Usage
-----

``jaas list-roles [options]``

.. _options-33:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--limit``                    | 0                     | The maximum number of roles to return     |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--offset``                   | 0                     | The offset to use when requesting roles   |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-33:

Examples
--------

::

   juju list-roles list

.. _details-34:

Details
-------

Lists all roles.

jaas set-controller-deprecated
==============================

.. _summary-35:

Summary
-------

Sets controller deprecated status.

.. _usage-35:

Usage
-----

``jaas set-controller-deprecated [options] <controller name>``

.. _options-34:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-34:

Examples
--------

::

   juju set-controller-deprecated mycontroller

.. _details-35:

Details
-------

Sets the deprecated status of a controller.

jaas unregister-controller
==========================

.. _summary-36:

Summary
-------

Remove controller from jimm

.. _usage-36:

Usage
-----

``jaas unregister-controller [options] <name>``

.. _options-35:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--force``                    | false                 | force unregister a controller             |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-35:

Examples
--------

::

   juju unregister-controller mycontroller 
   juju unregister-controller mycontroller --force

.. _details-36:

Details
-------

Deregisters a controller from JIMM.

jaas update-migrated-model
==========================

.. _summary-37:

Summary
-------

Update the controller running a model.

.. _usage-37:

Usage
-----

``jaas update-migrated-model [options] <controller name> <model uuid>``

.. _options-36:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-36:

Examples
--------

::

   juju update-migrated-model mycontroller e0bf3abf-7029-4e48-9c26-68a7b6e02947

.. _details-37:

Details
-------

Updates a model known to JIMM that has been migrated externally to a different JAAS controller.

jaas update-service-account-credential
======================================

.. _summary-38:

Summary
-------

Update service account cloud credential

.. _usage-38:

Usage
-----

``jaas update-service-account-credential [options] <client-id> <cloud> <credential-name>``

.. _options-37:

Options
~~~~~~~

+--------------------------------+-----------------------+-----------------------------------------------------------------------+
| Flag                           | Default               | Usage                                                                 |
+================================+=======================+=======================================================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication                             |
+--------------------------------+-----------------------+-----------------------------------------------------------------------+
| ``--client``                   | false                 | Provide this option to use a credential from your local store instead |
+--------------------------------+-----------------------+-----------------------------------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)                                     |
+--------------------------------+-----------------------+-----------------------------------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                                                |
+--------------------------------+-----------------------+-----------------------------------------------------------------------+

.. _examples-37:

Examples
--------

::

   juju update-service-account-credential <client-id> aws <credential-name>
   juju update-service-account-credential --client <client-id> aws <credential-name> 

.. _details-38:

Details
-------

update-service-account-credential command updates the credentials associated with a service account. Without any additional flags this command will search for the specified credentials on the controller and create a copy that belongs to the service account.

If the –client option is provided, the command will search for the specified credential on your local client store and upload a copy of the credential that will be owned by the service account.
