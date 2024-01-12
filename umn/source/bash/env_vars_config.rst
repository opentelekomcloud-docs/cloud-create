.. _env_vars_config:

**************************************
Environment variables for a connection
**************************************

The following environment variables are available in the operation **post_configure_source** and **add_source** of the script component:

* **SOURCE_NODE**: name of the source node in the relationship (e.g., :code:`Bash_1`).

* **TARGET_NODE**: name of the target node in the relationship (e.g., :code:`Bash_2`).

* **SOURCE_HOST**: name of the compute node which hosts the source node (e.g., :code:`Compute_1`).

* **TARGET_HOST**: name of the compute node which hosts the target node (e.g., :code:`Compute_2`).

* **SOURCE_IP** and **TARGET_IP**: the runtime IP address of the compute nodes, which host the source and target components, respectively.

* All properties of the components are also available. They are prefixed by :code:`SOURCE_` and :code:`TARGET_` (e.g., SOURCE_PORT, TARGET_PORT, etc.).

Examples
========

* Select :code:`Bash_2`.
* Set the :code:`port` property to :code:`27017`.
* Add an entry in the :code:`env` property with key :code:`FOO` value :code:`bar`.

.. figure:: /_static/images/7-Bash-env-relationship.png
  :width: 800

  Figure 1. Set environment variable on a target node

In **post_configure_source** of :code:`Bash_1`, we can access the environment variables of :code:`Bash_2`:

.. code-block:: bash

  # Result: Bash_2 is running on Compute_2 on port 27017
  echo "$TARGET_NODE is running on $TARGET_HOST on port $TARGET_PORT"

  # Result: Bash_2 has environment variable FOO with value: bar
  echo "$TARGET_NODE has environment variable FOO with value: $TARGET_FOO"
