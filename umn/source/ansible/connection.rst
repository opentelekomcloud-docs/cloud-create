************************************************************
How to configure a connection between two ansible components
************************************************************

* You can connect **AnsibleTask**, **AnsibleRole**, **AnsibleGalaxy**, and **Bash** component with each other.
* The operation **post_configure_source** and **add_source** are executed in the same order as described in Section :ref:`bash relationship`.
* The environment variables of the SOURCE node and TARGET node are available in the operations as described in :ref:`bash env vars`.

Examples
========

* Design the topology as belows.
* Connect the **AnsibleTasks** and the **Bash** component to the **AnsibleRole** component.

.. figure:: /_static/images/8-Ansible-tasks-relation.png
  :width: 800

  Figure 1. Ansible tasks relationship

.. tip:: You can find this example under New Application / Topology Template / Ansible-Bash-Connector.

* Select **AnsibleRole**.
* Set the :code:`port` property to :code:`27017`.
* Set an :code:`env` property with key :code:`FOO` value :code:`bar`.

.. figure:: /_static/images/8-Ansible-tasks-relation-env.png
  :width: 800

  Figure 2. Set environment variable on a target node

In the **post_configure_source** of the **AnsibleTasks** component, we can write a script to access the enrionment variables of the **AnsibleRole** component:

.. code-block:: yaml

  - debug:
      msg:
        - "Hello I am an ansible task {{ SOURCE_NODE }} executes on {{ SOURCE_HOST }} to connect with {{ TARGET_NODE }} at {{ TARGET_IP }}."
        - "I can access the env variable FOO on the target node {{ TARGET_NODE }} with value: {{ TARGET_FOO }}."

Expected result
===============

.. figure:: /_static/images/8-Ansible-tasks-post-configure.png
  :width: 800

  Figure 3. Deployment logs of post_configure_source