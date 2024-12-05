Jimmctl Reference
#################

Index
=====

0.  `add-cloud-to-controller <#add-cloud-to-controller>`__
1.  `add-controller <#add-controller>`__
2.  `audit-events <#audit-events>`__
3.  `auth <#auth>`__
4.  `controller-info <#controller-info>`__
5.  `controllers <#controllers>`__
6.  `grant-audit-log-access <#grant-audit-log-access>`__
7.  `import-cloud-credentials <#import-cloud-credentials>`__
8.  `import-model <#import-model>`__
9.  `list-audit-events <#list-audit-events>`__
10. `list-controllers <#list-controllers>`__
11. `migrate <#migrate>`__
12. `model-status <#model-status>`__
13. `purge-audit-logs <#purge-audit-logs>`__
14. `query-models <#query-models>`__
15. `remove-cloud-from-controller <#remove-cloud-from-controller>`__
16. `remove-controller <#remove-controller>`__
17. `revoke-audit-log-access <#revoke-audit-log-access>`__
18. `set-controller-deprecated <#set-controller-deprecated>`__
19. `update-migrated-model <#update-migrated-model>`__

--------------

ADD-CLOUD-TO-CONTROLLER
=======================

Summary
-------

Add cloud to specific controller in jimm

Usage
-----

``jimmctl add-cloud-to-controller [options] <controller_name> <cloud_name>``

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

   jimmctl add-cloud-to-controller mycontroller mycloud
   jimmctl add-cloud-to-controller mycontroller mycloud --cloud=./cloud-definition.yaml

Details
-------

The add-cloud-to-controller command adds the specified cloud to a specific controller on jimm.

One can specify a cloud definition via a yaml file passed with the –cloud flag. If the flag is missing, the command will assume the cloud definition is already known and will error otherwise.

ADD-CONTROLLER
==============

.. _summary-1:

Summary
-------

Add controller to jimm

.. _usage-1:

Usage
-----

``jimmctl add-controller [options] <filepath>``

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

   jimmctl add-controller ./controller-info 
   jimmctl add-controller ./controller-info.yaml --format json

.. _details-1:

Details
-------

The add-controller command adds a controller to jimm.

AUDIT-EVENTS
============

**Aliases:** audit-events

.. _summary-2:

Summary
-------

Displays audit events

.. _usage-2:

Usage
-----

``jimmctl list-audit-events [options]``

.. _options-2:

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

.. _examples-2:

Examples
--------

::

   jimmctl list-audit-events --after 2020-01-01T15:00:00 --before 2020-01-01T15:00:00 --user-tag user@canonical.com --limit 50
   jimmctl list-audit-events --method CreateModel
   jimmctl audit-events --after 2020-01-01T15:00:00 --format yaml

.. _details-2:

Details
-------

The list-audit-events command displays matching audit events.

AUTH
====

.. _summary-3:

Summary
-------

Authorisation model management.

.. _usage-3:

Usage
-----

``jimmctl auth [flags] <command> ...``

.. _options-3:

Options
~~~~~~~

================== ======= ========================================
Flag               Default Usage
================== ======= ========================================
``--description``  false   Show short description of plugin, if any
``-h``, ``--help`` false   Show help on a command or other topic.
================== ======= ========================================

.. _details-3:

Details
-------

The auth command enables users to manage authorisation model used by JIMM.

Subcommands
-----------

- `group <#auth-group>`__
- `relation <#auth-relation>`__
- `role <#auth-role>`__

AUTH GROUP
==========

.. _summary-4:

Summary
-------

Group management.

.. _usage-4:

Usage
-----

``jimmctl auth group [options] <command> ...``

.. _options-4:

Options
~~~~~~~

================== ======= ========================================
Flag               Default Usage
================== ======= ========================================
``--description``  false   Show short description of plugin, if any
``-h``, ``--help`` false   Show help on a command or other topic.
================== ======= ========================================

.. _details-4:

Details
-------

The group command enables group management for jimm

.. _subcommands-1:

Subcommands
-----------

- `add <#auth-group-add>`__
- `list <#auth-group-list>`__
- `remove <#auth-group-remove>`__
- `rename <#auth-group-rename>`__

AUTH GROUP ADD
==============

.. _summary-5:

Summary
-------

Add group to jimm.

.. _usage-5:

Usage
-----

``jimmctl auth group add [options] <name>``

.. _options-5:

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

   jimmctl auth group add mygroup

.. _details-5:

Details
-------

The add command adds group to jimm.

AUTH GROUP LIST
===============

.. _summary-6:

Summary
-------

List all groups.

.. _usage-6:

Usage
-----

``jimmctl auth group list [options]``

.. _options-6:

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

.. _examples-4:

Examples
--------

::

   jimmctl auth group list

.. _details-6:

Details
-------

The list command lists all groups in jimm.

AUTH GROUP REMOVE
=================

.. _summary-7:

Summary
-------

Remove a group.

.. _usage-7:

Usage
-----

``jimmctl auth group remove [options] <name>``

.. _options-7:

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

.. _examples-5:

Examples
--------

::

   jimmctl auth group remove mygroup

.. _details-7:

Details
-------

The remove command removes a group in jimm.

AUTH GROUP RENAME
=================

.. _summary-8:

Summary
-------

Rename a group.

.. _usage-8:

Usage
-----

``jimmctl auth group rename [options] <name> <new name>``

.. _options-8:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-6:

Examples
--------

::

   jimmctl auth group rename mygroup newgroup

.. _details-8:

Details
-------

The rename command renames a group in jimm.

AUTH RELATION
=============

.. _summary-9:

Summary
-------

Relation management.

.. _usage-9:

Usage
-----

``jimmctl auth relation [options] <command> ...``

.. _options-9:

Options
~~~~~~~

================== ======= ========================================
Flag               Default Usage
================== ======= ========================================
``--description``  false   Show short description of plugin, if any
``-h``, ``--help`` false   Show help on a command or other topic.
================== ======= ========================================

.. _details-9:

Details
-------

relation command enables relation management for jimm

.. _subcommands-2:

Subcommands
-----------

- `add <#auth-relation-add>`__
- `check <#auth-relation-check>`__
- `list <#auth-relation-list>`__
- `remove <#auth-relation-remove>`__

AUTH RELATION ADD
=================

.. _summary-10:

Summary
-------

Add relation to jimm.

.. _usage-10:

Usage
-----

``jimmctl auth relation add [options] <object> <relation> <target_object>``

.. _options-10:

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

.. _examples-7:

Examples
--------

::

   jimmctl auth relation add user-alice@canonical.com member group-mygroup
   jimmctl auth relation add group-MyTeam#member admin model-mymodel
   jimmctl auth relation add -f /path/to/file.yaml

.. _details-10:

Details
-------

The add command adds relation to jimm.

The object and target object must be of the form <tag>-<objectname> or <tag>-<object-uuid> E.g. “user-Alice” or “controller-MyController”

-f Read from a file where filename is the location of a JSON encoded file of the form: [ { “object”:“user-mike”, “relation”:“member”, “target_object”:“group-yellow” }, { “object”:“user-alice”, “relation”:“member”, “target_object”:“group-yellow” } ]

Certain constraints apply when creating/removing a relation, namely: Object may be one of:

::

   user tag                = "user-&lt;name&gt;"
   group tag               = "group-&lt;name&gt;"
   controller tag          = "controller-&lt;name&gt;"
   model tag               = "model-&lt;name&gt;"
   application offer tag   = "offer-&lt;name&gt;"

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

Additionally, if the object is a group, a userset can be applied by adding #member as follows. This will grant/revoke the relation to all users within TeamA:

::

   group-TeamA#member administrator controller-MyController

AUTH RELATION CHECK
===================

.. _summary-11:

Summary
-------

Check access to a resource.

.. _usage-11:

Usage
-----

``jimmctl auth relation check [options] <object> <relation> <target_object>``

.. _options-11:

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

.. _examples-8:

Examples
--------

::

   jimmctl auth relation check user-alice@canonical.com administrator controller-aws-controller-1

.. _details-11:

Details
-------

Verifies the access between resources.

AUTH RELATION LIST
==================

.. _summary-12:

Summary
-------

List relations.

.. _usage-12:

Usage
-----

``jimmctl auth relation list [options]``

.. _options-12:

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

.. _examples-9:

Examples
--------

List all relations

::

   jimmctl auth relation list

List relations where the target object match

::

   jimmctl auth relation list --target model-mymodel

List relations where the target object and relation match

::

   jimmctl auth relation list --target model-mymodel  --relation admin

.. _details-12:

Details
-------

List relations known to jimm. Using the “target”, “relation” and “object” flags, only those relations matching the filter will be returned.

AUTH RELATION REMOVE
====================

.. _summary-13:

Summary
-------

Remove relation from jimm.

.. _usage-13:

Usage
-----

``jimmctl auth relation remove [options] <object> <relation> <target_object>``

.. _options-13:

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

.. _examples-10:

Examples
--------

::

   jimmctl auth relation remove user-alice@canonical.com member group-mygroup
   jimmctl auth relation remove group-MyTeam#member admin model-mymodel
   jimmctl auth relation remove -f /path/to/file.yaml

.. _details-13:

Details
-------

The remove command removes a relation from jimm.

The object and target object must be of the form <tag>-<objectname> or <tag>-<object-uuid> E.g. “user-Alice” or “controller-MyController”

-f Read from a file where filename is the location of a JSON encoded file of the form: [ { “object”:“user-mike”, “relation”:“member”, “target_object”:“group-yellow” }, { “object”:“user-alice”, “relation”:“member”, “target_object”:“group-yellow” } ]

Certain constraints apply when creating/removing a relation, namely: Object may be one of:

::

   user tag                = "user-&lt;name&gt;"
   group tag               = "group-&lt;name&gt;"
   controller tag          = "controller-&lt;name&gt;"
   model tag               = "model-&lt;name&gt;"
   application offer tag   = "offer-&lt;name&gt;"

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

Additionally, if the object is a group, a userset can be applied by adding #member as follows. This will grant/revoke the relation to all users within TeamA:

::

   group-TeamA#member administrator controller-MyController

AUTH ROLE
=========

.. _summary-14:

Summary
-------

Role management.

.. _usage-14:

Usage
-----

``jimmctl auth role [options] <command> ...``

.. _options-14:

Options
~~~~~~~

================== ======= ========================================
Flag               Default Usage
================== ======= ========================================
``--description``  false   Show short description of plugin, if any
``-h``, ``--help`` false   Show help on a command or other topic.
================== ======= ========================================

.. _details-14:

Details
-------

The role command enables role management for jimm

.. _subcommands-3:

Subcommands
-----------

- `add <#auth-role-add>`__
- `list <#auth-role-list>`__
- `remove <#auth-role-remove>`__
- `rename <#auth-role-rename>`__

AUTH ROLE ADD
=============

.. _summary-15:

Summary
-------

Add role to jimm.

.. _usage-15:

Usage
-----

``jimmctl auth role add [options] <role name>``

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
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-11:

Examples
--------

::

   jimmctl auth role add myrole 

.. _details-15:

Details
-------

The add command adds role to jimm.

AUTH ROLE LIST
==============

.. _summary-16:

Summary
-------

List all roles.

.. _usage-16:

Usage
-----

``jimmctl auth role list [options]``

.. _options-16:

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

.. _examples-12:

Examples
--------

::

   jimmctl auth role list

.. _details-16:

Details
-------

The list command lists all roles in jimm.

AUTH ROLE REMOVE
================

.. _summary-17:

Summary
-------

Remove a role.

.. _usage-17:

Usage
-----

``jimmctl auth role remove [options] <role name>``

.. _options-17:

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

.. _examples-13:

Examples
--------

::

   jimmctl auth role remove myrole

.. _details-17:

Details
-------

The remove command removes a role in jimm.

AUTH ROLE RENAME
================

.. _summary-18:

Summary
-------

Rename a role.

.. _usage-18:

Usage
-----

``jimmctl auth role rename [options] <role name> <new role name>``

.. _options-18:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-14:

Examples
--------

::

   jimmctl auth role rename myrole newrolename

.. _details-18:

Details
-------

The rename command renames a role in jimm.

CONTROLLER-INFO
===============

.. _summary-19:

Summary
-------

Stores controller info to a yaml file

.. _usage-19:

Usage
-----

``jimmctl controller-info [options] <name> <filepath> [<public address>]``

.. _options-19:

Options
~~~~~~~

+--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------+
| Flag                           | Default               | Usage                                                                                              |
+================================+=======================+====================================================================================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication                                                          |
+--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------+
| ``--local``                    | false                 | If local flag is specified, then the local API address and CA cert of the controller will be used. |
+--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------+
| ``--tls-hostname``             |                       | Specify the hostname for TLS verification.                                                         |
+--------------------------------+-----------------------+----------------------------------------------------------------------------------------------------+

.. _examples-15:

Examples
--------

::

   jimmctl controller-info mycontroller ./destination/file.yaml mycontroller.example.com 
   jimmctl controller-info mycontroller ./destination/file.yaml --local

.. _details-19:

Details
-------

The controller-info command writes controller information contained in the juju client store to a yaml file.

If a public address is specified, the output controller information will contain the public address provided and omit a CA cert, this assumes that the server is secured with a public certificate.

Use the –local flag if the server is not configured with a public address.

CONTROLLERS
===========

**Aliases:** list-controllers

.. _summary-20:

Summary
-------

Lists all controllers known to JIMM.

.. _usage-20:

Usage
-----

``jimmctl controllers [options]``

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

.. _examples-16:

Examples
--------

::

   jimmctl controllers 
   jimmctl controllers --format json

.. _details-20:

Details
-------

The list-controllers command displays controller information for all controllers known to JIMM.

DOCUMENTATION
=============

.. _summary-21:

Summary
-------

Generate the documentation for all commands

.. _usage-21:

Usage
-----

``jimmctl documentation [options] --out <target-folder> --no-index --split --url <base-url> --discourse-ids <filepath>``

.. _options-21:

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

.. _examples-17:

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

.. _details-21:

Details
-------

This command generates a markdown formatted document with all the commands, their descriptions, arguments, and examples.

GRANT-AUDIT-LOG-ACCESS
======================

.. _summary-22:

Summary
-------

Grants access to audit logs.

.. _usage-22:

Usage
-----

``jimmctl grant-audit-log-access [options] <username>``

.. _options-22:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-18:

Examples
--------

::

   jimmctl grant-audit-log-access <username> 

.. _details-22:

Details
-------

Grants a user access to read audit logs.

HELP
====

.. _summary-23:

Summary
-------

Show help on a command or other topic.

.. _usage-23:

Usage
-----

``jimmctl help [flags] [topic]``

.. _details-23:

Details
-------

See also: topics

IMPORT-CLOUD-CREDENTIALS
========================

.. _summary-24:

Summary
-------

Import cloud credentials to jimm

.. _usage-24:

Usage
-----

``jimmctl import-cloud-credentials [options] <filepath>``

.. _options-23:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-19:

Examples
--------

::

   jimmctl import-cloud-credentials ./path/creds.json

.. _details-24:

Details
-------

The import-cloud-credentials imports a set of cloud credentials loaded from a file containing a series of JSON objects. The JSON objects specifying the credentials should be of the form:

{ “\_id”: <cloud-credential-id>, “type”: <credential-type>, “attributes”: { <key1>: <value1>, … } }

IMPORT-MODEL
============

.. _summary-25:

Summary
-------

Import a model to jimm

.. _usage-25:

Usage
-----

``jimmctl import-model [options] <controller name> <model uuid>``

.. _options-24:

Options
~~~~~~~

+--------------------------------+-----------------------+--------------------------------------------+
| Flag                           | Default               | Usage                                      |
+================================+=======================+============================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication  |
+--------------------------------+-----------------------+--------------------------------------------+
| ``--owner``                    |                       | switch the model owner to the desired user |
+--------------------------------+-----------------------+--------------------------------------------+

.. _examples-20:

Examples
--------

::

   jimmctl import-model mycontroller ac30d6ae-0bed-4398-bba7-75d49e39f189
   jimmctl import-model mycontroller ac30d6ae-0bed-4398-bba7-75d49e39f189 --owner user@canonical.com

.. _details-25:

Details
-------

The import-model imports a model running on a controller to jimm.

When importing, it is necessary for JIMM to contain a set of cloud credentials that represent a user’s access to the incoming model’s cloud.

The –owner command is necessary when importing a model created by a local user and it will switch the model owner to the desired external user.

LIST-AUDIT-EVENTS
=================

**Aliases:** audit-events

.. _summary-26:

Summary
-------

Displays audit events

.. _usage-26:

Usage
-----

``jimmctl list-audit-events [options]``

.. _options-25:

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

.. _examples-21:

Examples
--------

::

   jimmctl list-audit-events --after 2020-01-01T15:00:00 --before 2020-01-01T15:00:00 --user-tag user@canonical.com --limit 50
   jimmctl list-audit-events --method CreateModel
   jimmctl audit-events --after 2020-01-01T15:00:00 --format yaml

.. _details-26:

Details
-------

The list-audit-events command displays matching audit events.

LIST-CONTROLLERS
================

**Aliases:** list-controllers

.. _summary-27:

Summary
-------

Lists all controllers known to JIMM.

.. _usage-27:

Usage
-----

``jimmctl controllers [options]``

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

.. _examples-22:

Examples
--------

::

   jimmctl controllers 
   jimmctl controllers --format json

.. _details-27:

Details
-------

The list-controllers command displays controller information for all controllers known to JIMM.

MIGRATE
=======

.. _summary-28:

Summary
-------

Migrate models to the target controller

.. _usage-28:

Usage
-----

``jimmctl migrate [options] <controller name> <model uuid> [<model uuid>...]``

.. _options-27:

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

.. _examples-23:

Examples
--------

::

   jimmctl migrate mycontroller 2cb433a6-04eb-4ec4-9567-90426d20a004 
   jimmctl migrate mycontroller 2cb433a6-04eb-4ec4-9567-90426d20a004 fd469983-27c2-423b-bebf-84f616fb036b ...

.. _details-28:

Details
-------

The migrate command migrates a model(s) to a new controller. Specify a model-uuid to migrate and the destination controller name.

Note that multiple models can be targeted for migration by supplying multiple model uuids.

MODEL-STATUS
============

.. _summary-29:

Summary
-------

Displays full model status

.. _usage-29:

Usage
-----

``jimmctl model-status [options] <model uuid>``

.. _options-28:

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

.. _examples-24:

Examples
--------

::

   jimmctl model-status 2cb433a6-04eb-4ec4-9567-90426d20a004 
   jimmctl model-status 2cb433a6-04eb-4ec4-9567-90426d20a004 --format yaml

.. _details-29:

Details
-------

The model-status command displays full model status.

PURGE-AUDIT-LOGS
================

.. _summary-30:

Summary
-------

purge audit logs from the database before the given date

.. _usage-30:

Usage
-----

``jimmctl purge-audit-logs [options] <date>``

.. _options-29:

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

.. _examples-25:

Examples
--------

::

   jimmctl purge-audit-logs 2021-02-03
   jimmctl purge-audit-logs 2021-02-03T00
   jimmctl purge-audit-logs 2021-02-03T15:04:05Z   

.. _details-30:

Details
-------

The purge-audit-logs purges logs from the database before the given date.

The provided date must be formatted as an ISO8601 date string.

QUERY-MODELS
============

.. _summary-31:

Summary
-------

Query model statuses

.. _usage-31:

Usage
-----

``jimmctl query-models [options] <query>``

.. _options-30:

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

.. _examples-26:

Examples
--------

::

   jimmctl query-models '.applications | with_entries(select(.key=="nginx-ingress-integrator"))'

.. _details-31:

Details
-------

The query-models command queries all models available to the current user performing the query against each model status individually, returning the collated query responses for each model.

The query runs against the output of “juju status –format json”, as such you can format your query against an output like this.

The queries expect a JQ query string.

REMOVE-CLOUD-FROM-CONTROLLER
============================

.. _summary-32:

Summary
-------

Remove cloud from specific controller in jimm

.. _usage-32:

Usage
-----

``jimmctl remove-cloud-from-controller [options] <controller_name> <cloud_name>``

.. _options-31:

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

.. _examples-27:

Examples
--------

::

   jimmctl remove-cloud-from-controller mycontroller mycloud

.. _details-32:

Details
-------

The remove-cloud-from-controller command removes the specified cloud from the specified controller in jimm.

REMOVE-CONTROLLER
=================

.. _summary-33:

Summary
-------

Remove controller from jimm

.. _usage-33:

Usage
-----

``jimmctl remove-controller [options] <name>``

.. _options-32:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--force``                    | false                 | force remove a controller                 |
+--------------------------------+-----------------------+-------------------------------------------+
| ``--format``                   | yaml                  | Specify output format (json|yaml)         |
+--------------------------------+-----------------------+-------------------------------------------+
| ``-o``, ``--output``           |                       | Specify an output file                    |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-28:

Examples
--------

::

   jimmctl remove-controller mycontroller 
   jimmctl remove-controller mycontroller --force

.. _details-33:

Details
-------

The remove-controller command removes a controller from jimm.

REVOKE-AUDIT-LOG-ACCESS
=======================

.. _summary-34:

Summary
-------

revokes access to audit logs.

.. _usage-34:

Usage
-----

``jimmctl revoke-audit-log-access [options] <user>``

.. _options-33:

Options
~~~~~~~

+--------------------------------+-----------------------+-------------------------------------------+
| Flag                           | Default               | Usage                                     |
+================================+=======================+===========================================+
| ``-B``, ``--no-browser-login`` | false                 | Do not use web browser for authentication |
+--------------------------------+-----------------------+-------------------------------------------+

.. _examples-29:

Examples
--------

::

   jimmctl revoke-audit-log-access user@canonical.com

.. _details-34:

Details
-------

The revoke-audit-log-access revokes user access to audit logs.

SET-CONTROLLER-DEPRECATED
=========================

.. _summary-35:

Summary
-------

Sets controller deprecated status.

.. _usage-35:

Usage
-----

``jimmctl set-controller-deprecated [options] <controller name>``

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

.. _examples-30:

Examples
--------

::

   jimmctl set-controller-deprecated mycontroller

.. _details-35:

Details
-------

The set-controller-deprecated sets the deprecated status of a controller.

UPDATE-MIGRATED-MODEL
=====================

.. _summary-36:

Summary
-------

Update the controller running a model.

.. _usage-36:

Usage
-----

``jimmctl update-migrated-model [options] <controller name> <model uuid>``

.. _options-35:

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

   jimmctl update-migrated-model mycontroller e0bf3abf-7029-4e48-9c26-68a7b6e02947

.. _details-36:

Details
-------

The update-migrated-model updates a model known to JIMM that has been migrated externally to a different JAAS controller.
