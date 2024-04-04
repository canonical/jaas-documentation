JAAS: Cross-Model Queries
=============================

Introduction
------------

In this how-to, we will perform some cross-model queries via JIMM.

Prerequisites
-------------

This feature is currently only available via the CLI. The following prerequisites are required to follow this how-to.

For this tutorial you will need the following:

- A deployed JIMM
- A controller registered with JIMM

Sending Queries
---------------
Queries run over each model that the user has available to them.

The queries are sent in ``jq`` format, and run over a string identical to the output of:

``juju status --format json``

So, if you want to practice querying model statuses, you can do it locally and expect the same to work when using JIMM's cross-model queries.

Each query response has an ``errors`` and ``results`` field, containing the resulting query responses for each model available to the current user. The errors field may return errors where the ``jq`` query string is incorrectly formatted, or something went wrong internally within JIMM.

The responses will contain an JSON array map of ``[model-uuid]:[query response JSON]``.

Example:
--------
Run the following to add test models::

     juju add-model test microk8s
     juju add-model test-2 microk8s
     juju add-model test-3 microk8s

Using ``jimmctl``, we can query for a key that does not exist like so:

``./jimmctl query-models .noneExistentKey | jq``

And the output will look like::

     {
     "results": {
         "e0c5c2fe-c00a-4013-8e69-7a168c86365f": [
         null
         ],
         "e5f83f7a-99e0-4383-8369-431d4ddc3a43": [
         null
         ],
         "f776ca74-2e21-4af2-83a0-d90ede581c6c": [
         null
         ]
     },
     "errors": {}
     }

Next, we'll perform a valid query, searching specifically for a model by name.
Note we wrap the query in single quotes here, so that the parenthesis aren't interpreted by bash:

``./jimmctl query-models 'select(.model.name=="test-2")' | jq``

We currently have a MicroK8s controller registered from earlier, and as we can see, the model details for that model have been returned.::

    {
    "results": {
        "f776ca74-2e21-4af2-83a0-d90ede581c6c": [
        {
            "applications": {},
            "controller": {
            "timestamp": "12:25:28"
            },
            "machines": {},
            "model": {
            "cloud": "microk8s",
            "controller": "",
            "model-status": {
                "current": "available",
                "since": "2023-03-31 12:19:31Z"
            },
            "name": "test-2",
            "region": "localhost",
            "sla": "unsupported",
            "type": "caas",
            "version": "2.9.42"
            }
        }
        ]
    },
    "errors": {}
    }

And that's a wrap! Below, you'll find some “useful queries” that may be helpful!

Useful Queries
--------------

Below are some predefined and potentially useful queries:

- ``.applications`` - Get all applications.
- ``.model`` - Get all models.
- ``.model | select(."model-status".current=="available")`` - Get all models that are currently available.
- ``select(.applications | to_entries[] | select(.key=="traefik")) | .model`` - Return all models where they contain an application of the name ``traefik``.
- ``.applications | to_entries[] | select(.key=="traefik" and .value."application-status".current=="waiting") | .value`` - Get all applications where their name is ``traefik`` and their current status is “waiting”.
- ``.applications | select(.[].units | select(. != null) | . as $charms_with_units | to_entries[] | .value as $charms_with_units | $charms_with_units.subordinates | to_entries[] | .key | match("landscape-client\/\\d+"))`` - Get all applications with a landscape-client subordinate (change app name for subordinate to get another kind).
