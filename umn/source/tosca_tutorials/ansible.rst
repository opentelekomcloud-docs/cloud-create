*********************************************************
How to define a service catalog using an ansible playbook
*********************************************************

In the previous example, we used a python script to implement a software catalog. In this example, we use an ansible playbook to create a mongodb.

.. figure:: /_static/images/tosca-tutorial/tosca-mongodb.png

  Figure 1. Mongodb example

1. Steps
========

Step 1. Import TOSCA types
--------------------------

.. code-block:: yaml

  imports:
    - tosca-normative-types:1.0.0-ALIEN20

Step 2. Implement the interfaces
--------------------------------

Implement the node :code:`MongoDB` with the :code:`create` interfaces and provide an ansible script for the interface (e.g., :code:`mongodb_install.yaml`):

.. code-block:: yaml

  node_types:
    myservice.nodes.SoftwareComponent.MongoDB:
      derived_from: tosca.nodes.SoftwareComponent
      ...
      interfaces:
        Standard:
          create:
            inputs:
              # HOST is the keyword to get more information about the hosted compute node at runtime
              IP_ADDRESS: { get_attribute: [HOST, private_address] }
              MONGODB_PORT: { get_property: [SELF, port] }
            # ansible script
            implementation: playbooks/mongodb_install.yaml

.. note::

  * Use the keyword :code:`HOST` to get information of the hosted compute node.
  * Use the function :code:`get_attribute` to get the runtime attribute :code:`private_address` of the hosted compute node. The compute node also has the following attributes available at runtime that you can use by default: :code:`private_address`, :code:`public_address`:

  .. figure:: /_static/images/tosca-tutorial/compute_attributes.png

    Figure 2. Runtime attributes of a compute node

Step 3. Implement an ansible playbook for the interface
-------------------------------------------------------

Write an ansible script (e.g., :code:`mongodb_install.yaml`), which imports a third party ansible role :code:`undergreen.ansible-role-mongodb` and uses it to install mongodb:

.. code-block:: yaml

  # playbooks/mongodb_install.yaml
  - name: Install MongoDB
    # "hosts" must be set to all. Otherwise the deployment will fail.
    hosts: all
    strategy: free
    become: true
    become_method: sudo
    tasks:
      - name: Install MongoDB using a 3rd party role
        import_role:
          name: undergreen.ansible-role-mongodb
        vars:
          # The environment variables IP_ADDRESS and MONGODB_PORT are the
          # input parameters of the create interface.
          mongodb_net_bindip: "{{ IP_ADDRESS }}"
          mongodb_net_port: "{{ MONGODB_PORT }}"
          ...

In the same folder of the ansible script, we have an ansible role available at :code:`playbooks/roles/undergreen.ansible-role-mongodb`

.. figure:: /_static/images/tosca-tutorial/mongodb_ansible_directory.png

  Figure 3. Playbook directory

Expected result
^^^^^^^^^^^^^^^

The orchestration engine will apply the ansible playbook :code:`mongodb_install.yaml` on the remote compute node, which in turn imports the ansible role :code:`undergreen.ansible-role-mongodb` to create the mongodb.

Step 3b. (alternative) Zip the whole playbook folder
----------------------------------------------------

Define ansible roles
^^^^^^^^^^^^^^^^^^^^

* In this scenario, we create a folder (e.g., :code:`playbook`) contaning more than one :code:`roles:code:` (e.g., :code:`create`, :code:`configure`, :code:`delete`, :code:`start`, and :code:`stop`).
* Each roles has an entry script (e.g., the entry script :code:`create.yml` imports the role :code:`create`).

.. figure:: /_static/images/tosca-tutorial/zip_playbook.png

  Figure 4. The entry script create.yml

Zip the playbook folder
^^^^^^^^^^^^^^^^^^^^^^^

Zip the **content** of the :code:`playbook` folder into the file :code:`playbook/playbook.ansible`

.. code-block:: bash

  cd playbook && rm -f playbook.ansible && zip -r playbook.ansible *

Implement the interface
^^^^^^^^^^^^^^^^^^^^^^^

In the :code:`create` interfaces, we specify the zip file as an implementation and set the :code:`PLAYBOOK_ENTRY` to the entry script :code:`create.yml`:

.. code-block:: yaml

  interfaces:
    Standard:
      create:
        inputs:
          # the entry script in the playbook folder
          PLAYBOOK_ENTRY: create.yml
        # the zip file of the playbook folder content
        implementation: playbook/playbook.ansible

Step 4. Define the outputs (optional)
-------------------------------------

* In the ansible script, use :code:`set_fact` to define an output:

.. code-block:: yaml

  - name: my tasks
    hosts: all
    tasks:
      - name: Set an output
        set_fact:
          OUTPUT: "A result of my tasks"

Expected result
^^^^^^^^^^^^^^^

The orchestration engine will apply the entry script :code:`create.yml`, which in turn imports the role :code:`create` (in the zip file) to create the component on the remote compute.

2. Known limitations
====================

2.1. Output a comma character
-----------------------------

* The output is a string but cannot contain a comma character :code:`,`. For a workaround, use encode:

.. code-block:: yaml

  set_fact:
    # ca.cert may contain a comma, so we encode it
    CA_CERT: "{{ lookup('file', '/tmp/ca.cert') | b64encode }}"

2.2. Output with become true
----------------------------

* The orchestrator cannot read the output if :code:`set_fact` is used together with :code:`become: true`:

.. code-block:: yaml

  - name: my tasks
    hosts: all
    # cannot get output
    become: true
    tasks:
      - name: Set an output
        set_fact:
          OUTPUT: "A result of my tasks"

* For a workaround, use :code:`become: true` in a task where you need it:

.. code-block:: yaml

  - name: my tasks
    hosts: all
    tasks:
      - name: A task with become true
        become: true

      - name: Set an output without become true
        set_fact:
          OUTPUT: "A result of my tasks"

3. Links
========

* See `full example how to use ansible script <https://github.com/opentelekomcloud-blueprints/tosca-tutorials/blob/master/examples/mongodb/types.yaml>`_
* See `full example how to use playbook as a zip file <https://github.com/opentelekomcloud-blueprints/tosca-tutorials/blob/master/examples/apache/types.yml>`_