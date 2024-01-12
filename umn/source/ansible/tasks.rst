*************
Ansible tasks
*************

Step 1. Drop
============

* Drop the **AnsibleTasks** component on a compute.

.. figure:: /_static/images/8-Ansible-tasks-configure.png
  :width: 800

  Figure 1. Drop an ansible task on a compute

Step 2. Write an Ansible task
=============================

1. Click on the **AnsibleTasks** component to open the code editor.
2. Copy and paste the following script in the code editor and **Save File**.

.. code-block:: yaml

  - debug:
      msg:
        - "{{ NODE }} is running on {{ HOST }} on port {{ PORT }}"
        - "{{ NODE }} has environment variable FOO with value: {{ FOO }}"

Step 3. Set Ansible variables
=============================

* All component properties are available as Ansible variables (e.g., set the **port** property to :code:`8080`).
* Use the property **env** to define any ansible variables (e.g., set the **env** property with key :code:`FOO` value :code:`bar`).
* Set **ansible_become** to :code:`true` to run the ansible tasks with root privileges. Defaults to :code:`false`.
* Set **ignore_errors** to :code:`true` for the orchestrator to ignore any errors during the execution of this ansible tasks and continues. Defaults to :code:`false`.

.. figure:: /_static/images/8-Ansible-tasks.png
  :width: 800

  Figure 2. Set ansible environment variables

Expected result
===============

During the deployment, the orchestrator executes the ansible tasks on the target Compute and prints out:

.. code-block:: bash

  AnsibleTasks is running on Compute on port 8080
  AnsibleTasks has environment variable FOO with value: bar
