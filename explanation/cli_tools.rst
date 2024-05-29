Understanding ``juju`` vs ``jaas`` vs ``jimmctl``
=================================================

This document aims to explain the differences between the various CLI tools you may encounter when using Juju and JAAS.

=======
Summary
=======

.. list-table::
   :widths: 25 25 25 25
   :header-rows: 0

   * - 
     - ``juju``
     - ``jaas``
     - ``jimmctl``
   * - Purpose
     - Used to interact with Juju/JIMM controllers
     - A plugin to add more commands to the Juju CLI
     - A tool for admins of the JIMM server
   * - Installation Link
     - `Snap package <https://snapcraft.io/juju>`__
     - `Snap package <https://snapcraft.io/jaas>`__
     - `Snap package <https://snapcraft.io/jimmctl>`__
   * - Use
     - ``juju <command>``
     - ``juju jaas -h`` and ``juju <command>``
     - ``jimmctl <command>``

``juju``
========

The Juju ecosystem introduces the Juju CLI as the first way a user can interact with their Juju environment.

The Juju CLI is provided as `a Snap <https://snapcraft.io/juju>`__ and allows you to communicate with a Juju controller.  

More information on the Juju CLI is available `here <https://juju.is/docs/juju/juju-client>`__.  
A full list of the Juju CLI commands is available `here <https://juju.is/docs/juju/juju-cli-commands>`__

The Juju CLI shares the same name as the Juju project so it's helpful to understand via context or via the explicit use of 
``juju`` when we are talking about the Juju CLI versus the Juju project.

``jaas``
========

The JAAS ecosystem introduces the JIMM controller. A special controller that sits in front of your Juju controllers 
and acts as an authorisation gateway and aggregator.

Interacting with the JIMM controller is done in the same fashion as Juju controllers, i.e. one uses the Juju CLI.  
However, JIMM offers some extra functionality and that is where ``jaas`` comes in.

The ``jaas`` CLI tool acts as a plugin for the Juju CLI. It is distributed as `a Snap <https://snapcraft.io/jaas>`__.  
When you install both the Juju and JAAS snaps, they automatically connect via 
`content-interface <https://snapcraft.io/docs/content-interface>`__ enabling new commands on the Juju CLI.

To view a list of all the newly available commands run ``juju jaas -h``.

These commands are intended to be used by all users of JAAS, giving you more capability with Juju.  

``jimmctl``
===========

As mentioned above, at the centre of the JAAS ecosystem is the JIMM controller. The ``jimmctl`` CLI tool is intended for administrators
of JIMM, to be used to debug permission issues, add controllers, access audit logs, etc.

The ``jimmctl`` tool is also distributed as `a Snap <https://snapcraft.io/jimmctl>`__.  
Use of the ``jimmctl`` tool requires that you are logged into the JIMM controller with the Juju CLI and that your user has administrator
permissions on the controller. At this point you can run ``jimmctl`` to interact with the system.

Try running ``jimmctl list-controllers`` to see which controllers are connected to JIMM.
