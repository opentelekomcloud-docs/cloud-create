*********************************************************
How to define a service catalog using an ansible playbook
*********************************************************

In the previous example, we used a python script to implement a software catalog. In this example, we use Ansible to create Apache server.

.. figure:: /_static/images/tosca-tutorial/tosca-apache.png
  :width: 800

  Figure 1. Apache example

1. Steps
========

Step 1. Import TOSCA types
--------------------------

.. code-block:: yaml

  imports:
    - tosca-normative-types:1.0.0-ALIEN20

Step 2. Implement the interfaces
--------------------------------

Implement the node :code:`Apache` with the :code:`create` interfaces and provide an Ansible playbook for the interface (e.g., :code:`apache-create.yaml`):

.. code-block:: yaml

  node_types:
    myservice.nodes.SoftwareComponent.Apache:
      derived_from: tosca.nodes.SoftwareComponent
      ...
      interfaces:
        Standard:
          create:
            inputs:
              # HOST is the keyword to get more information about the hosted compute node at runtime
              APACHE_LISTEN_IP: { get_attribute: [HOST, private_address] }
            # ansible script
            implementation: playbooks/apache-create.yaml

.. note::

  * Use the keyword :code:`HOST` to get information of the hosted compute node.
  * Use the function :code:`get_attribute` to get the runtime attribute :code:`private_address` of the hosted compute node. The compute node also has the following attributes available at runtime that you can use by default: :code:`private_address`, :code:`public_address`:

  .. figure:: /_static/images/tosca-tutorial/compute_attributes.png

    Figure 2. Runtime attributes of a compute node

Step 3. Implement an ansible playbook for the interface
-------------------------------------------------------

Write the Ansible playbook (e.g., :code:`apache-create.yaml`) to install Apache. In this example, we import a third party ansible role :code:`geerlingguy.apache` to install Apache:

.. code-block:: yaml

  # playbooks/apache-create.yaml
  - name: Install Apache
    # "hosts" must be set to all. Otherwise the deployment will fail.
    hosts: all
    tasks:
      - name: Install Apache
        become: true
        import_role:
          name: geerlingguy.apache
        vars:
          # The environment variable APACHE_LISTEN_IP is the input parameter of the create interface.
          apache_listen_ip: "{{ APACHE_LISTEN_IP }}"
          ...

In the same folder of the Ansible playbook, we have an Ansible role available at :code:`playbooks/roles/geerlingguy.apache`

.. figure:: /_static/images/tosca-tutorial/apache_ansible_directory.png
  :width: 300

  Figure 3. Playbook directory

Expected result
^^^^^^^^^^^^^^^

The orchestration engine will apply the ansible playbook :code:`apache-create.yaml` on the remote compute node, which in turn uses the Ansible role :code:`geerlingguy.apache` to create Apache.

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

* See `full example how to use ansible script <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs/blob/main/apache/types.yml>`_