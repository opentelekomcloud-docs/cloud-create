.. _bash relationship:

*********************************************************
How to configure a connection between two Bash components
*********************************************************

In the topology, you can configure a :code:`SOURCE` node (e.g., a web application) to connect to a :code:`TARGET` node (e.g., a database ) as follows:

Step 1. Drop
============

* Drop two **Bash** components on two compute nodes (e.g., :code:`Bash_1` and :code:`Bash_2`).

Step 2. Connect
===============

* Click on the **connect_to_data_endpoint** point of :code:`Bash_1` and connect to the **data_endpoint** of :code:`Bash_2`:

.. figure:: /_static/images/7-Bash-connect-to.png
  :width: 800

  Figure 1. Connect two script components

In this example, :code:`Bash_1` and :code:`Bash_2:code:` are the :code:`SOURCE` node and the :code:`TARGET` node of the relationship, respectively.

Step 3. Configure the source node
=================================

You can configure :code:`Bash_1` to connect to :code:`Bash_2` as follows:

* Click on :code:`Bash_1`.
* Provide the artifact for the operation **post_configure_source**. This operation will be executed on :code:`Compute_1` after the operation **configure** is executed on :code:`Compute_1` successfully.

.. figure:: /_static/images/7-Bash-pre-configure.png
  :width: 800

  Figure 2. Provide post_configure_source

For example:

.. code-block:: bash

  #!/usr/bin/env bash

  # Result: "Hello I am a script Bash_1 executing on Compute_1 to connect with Bash_2 at 10.0.0.4"
  echo "Hello I am a script $SOURCE_NODE executing on $SOURCE_HOST to connect with $TARGET_NODE at $TARGET_IP"

In this example, we use some environment variables (e.g., :code:`SOURCE_IP`, :code:`TARGET_IP`), which resolve the IP addresses of the two compute nodes at runtime. This information is useful for establishing the connection between two components. For more environment variables, see Section :ref:`env_vars_config`.

Step 4. Configure the target node
=================================

In the reverse direction, you can configure :code:`Bash_2` to connect to :code:`Bash_1` as follows:

* Click on :code:`Bash_1`.
* Provide the artifact for the operation **add_source**. This operation will be executed on :code:`Compute_2` after :code:`Bash_1` has completed successfully.

.. figure:: /_static/images/7-Bash-add_source.png
  :width: 800

  Figure 3. Provide add_source artifact

For example:

.. code-block:: bash

  #!/usr/bin/env bash

  # Result: "Hello I am a script Bash_1. I have completed and running at 10.0.0.3""
  echo "Hello I am a script $SOURCE_NODE. I have completed and running at $SOURCE_IP"

This example shows we can provide a script to execute on the target node :code:`Compute_2` (e.g., to notify that :code:`Bash_1` is up and running at a given IP address :code:`10.0.0.3`).

Expected result
===============

During the deployment, the resulting workflow between :code:`Bash_1` and :code:`Bash_2` is as follows:

.. figure:: /_static/images/7-Bash-workflow.png
  :width: 800

  Figure 4. Orchestration workflow between source node and target node

* The orchestrator setups the TARGET node first (i.e., it calls the operation :code:`configure` of :code:`Bash_2` on :code:`Compute_2`).
* Afterwards, it setups the SOURCE node to connect to the TARGET node (i.e., it calls the operation :code:`configure` and :code:`post_configure_source` of :code:`Bash_1` on :code:`Compute_1`).
* After the SOURCE node has started successfully, the orchestrator notifies the TARGET node (i.e., it calls the operation :code:`add_source` of :code:`Bash_1` on :code:`Compute_2`).