.. _tosca relationship:

****************************************************************
How to define a ConnectsTo relationship between service catalogs
****************************************************************

This example shows how to define a relationship for NodeA (a :code:`SOURCE` node) to connect to NodeB (a :code:`TARGET`
node) as in Figure 1.

* We will define :code:`capabilities` on the NodeB containing all information for NodeA to setup a connection.
* We will define the :code:`relationship` interfaces to setup the connection.

Steps
=====

Step 1. Define an endpoint capability in the TARGET node (NodeB)
------------------------------------------------------------------

An endpoint capability contains information of a :code:`TARGET` node for a :code:`SOURCE` node to setup the connection.

In NodeB, we define the following :code:`capabilities` block:

.. code-block:: yaml

  node_types:
    otc.nodes.SoftwareComponent.NodeB:
      ...
      capabilities:
        db_endpoint:
          type: tosca.capabilities.Endpoint.Database

In this example, we defined a new capability :code:`db_endpoint` from the TOSCA type :code:`tosca.capabilities.Endpoint.Database`. The capability :code:`db_endpoint` will inherit all default properties from the TOSCA type (e.g., :code:`port`, :code:`protocol`, :code:`url_path`).

In the designer, users can specifiy values for the capability :code:`db_endpoint`. For example, they may set the :code:`port` to :code:`27017`.

.. tip::

  The :code:`tosca.capabilities.Endpoint` also has a runtime attribute :code:`ip_address`. The orchestrator will automatically set the IP address of the hosted compute node to this attribute. A :code:`SOURCE` node can use this runtime attribute to setup a connection.

Step 2. Define a requirement in the SOURCE node (NodeA)
------------------------------------------------------------

In the NodeA node, add the following :code:`requirements` block:

.. code-block:: yaml

  node_types:
    otc.nodes.NodeA:
      derived_from: tosca.nodes.WebApplication
      ...
      requirements:
        - db_endpoint:
            # NodeA requires a node that has the capability Endpoint.Database
            capability: tosca.capabilities.Endpoint.Database
            # NodeA uses this relationship to setup the connection (see step 3)
            relationship: otc.relationships.NodeAConnectToNodeB
            # (Optional) specifiy relationship instance one-to-one
            # it means, one NodeA has one NodeB
            occurrences: [1, 1]

Step 3: Define the relationship
-------------------------------

Define a new relationship :code:`otc.relationships.NodeAConnectToNodeB`, how NodeA setups the connection with NodeB:

.. code-block:: yaml

  relationship_types:
    otc.relationships.NodeAConnectToNodeB:
      derived_from: tosca.relationships.ConnectsTo
      interfaces:
        Configure:
          pre_configure_source:
            inputs:
              # The input NODEA_PORT gets the port property of NodeA
              NODEA_PORT: { get_property: [SOURCE, port] }
              # The input NODEB_PORT gets the port property of NodeB
              NODEB_PORT: { get_property: [TARGET, port] }
              # The input NODEB_IP gets the runtime attribute ip_address of NodeB
              NODEB_IP: { get_attribute: [TARGET, db_endpoint, ip_address] }
            implementation: scripts/setup-connection.sh

In the above example, we defined the interface :code:`pre_configure_source` by providing a shell script (e.g., :code:`setup-connection.sh`). The script uses the input parameters :code:`NODEA_PORT`, :code:`NODEB_PORT`, and :code:`NODEB_IP` to configure NodeA to connect to NodeB.

.. note::

  * Use the function :code:`get_property` to get a SOURCE or TARGET node property.
  * Use the function :code:`get_attribute` to get a runtime attribute of a SOURCE or TARGET node (e.g., ip_address).

2. Relationship interfaces
==========================

In addition to the interface :code:`pre_configure_source`, we have the following interfaces

.. figure:: /_static/images/tosca-tutorial/relationship_lifecycle.png
  :width: 800

  Figure 2. Relationship lifecycle

2.1. Interfaces executed on the TARGET node
-------------------------------------------

* :code:`pre_configure_target`: executes after the :code:`TARGET` node is created.
* :code:`post_configure_target`: executes after the :code:`TARGET` node is configured.
* :code:`add_source`: executes on :code:`TARGET` node, notifying that the :code:`SOURCE` node is up and running.

2.2. Interfaces executed on the SOURCE node
-------------------------------------------

* :code:`pre_configure_source` executes after the :code:`SOURCE` node is created.
* :code:`post_configure_source`: executes after the :code:`SOURCE` node is configured.
* :code:`add_target`: executes after the :code:`SOURCE` node is started.
* :code:`remove_target`: executes after the :code:`TARGET` node is removed.

.. note::

  * The :code:`TARGET` node is always up and running first before the :code:`SOURCE` node.
  * All runtime attributes of the :code:`SOURCE` node are not available until it is up and running (i.e., they are available in the :code:`add_source` interface). Therefore, to configure the :code:`TARGET` node with any runtime attributes of the :code:`SOURCE` node, you can use the :code:`add_source` interface.


3. Links
========

* See `full example <https://github.com/opentelekomcloud-blueprints/tosca-tutorials/blob/master/examples/nodecellar/types.yml>`_.