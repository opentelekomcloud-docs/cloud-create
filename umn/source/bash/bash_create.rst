*****************************************
How to execute a Bash script on a compute
*****************************************

Step 1. Drop
============

* Drop the **Bash** component on a compute (e.g., :code:`Bash_1`).

.. figure:: /_static/images/7-Bash-properties.png
  :width: 800

  Figure 1. Bash_1 on Compute_1

Step 2. Write a script
======================

1. Click on the **Bash** component to open the code editor.

.. figure:: /_static/images/7-Bash-configure.png
  :width: 800

  Figure 2. Configure a Bash component

2. Copy and paste the following script in the code editor and **Save File**.

.. code-block:: bash

  #!/usr/bin/env bash

  echo "Start configuring $NODE on compute $HOST"
  echo "I have the ip address: $ip_address"

In this example, :code:`$NODE`, :code:`$HOST`, and :code:`$ip_address` are pre-defined environment variables. They will be resolved to the component name (e.g., :code:`Bash_1`), the hosted compute (e.g., :code:`Compute_1`), and the IP address of the compute (e.g., :code:`10.0.0.3`), respectively at runtime.

.. tip:: For more environment variables, see Section :ref:`bash env vars`.

(Alternative) Upload a script
=============================

If you have already had a script, you can upload it from your local machine:

1. Go to **Artifacts** / **configure** artifact.
2. Upload a **Local file**.
3. **Select** the file you have uploaded (e.g., :code:`my_file.sh`).

.. figure:: /_static/images/7-Bash-configure-upload.png
  :width: 800

  Figure 3. Providing artifacts for a Bash component

Expected result
===============

During the deployment, the orchestrator executes :code:`Bash_1` on the :code:`Compute_1` and print out the logs:

.. code-block:: bash

  Start configuring Bash_1 on compute Compute_1
  I have the ip address: 10.0.0.3

Artifacts management
====================

* Go to **Archive content**. This section allows you to manage all file artifacts that you uploaded or created for your application.

.. figure:: /_static/images/7-Bash-archive-content.png
  :width: 800

  Figure 4. View all artifacts