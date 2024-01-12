.. _tosca relationship:

****************************************************************
How to define a ConnectsTo relationship between service catalogs
****************************************************************

This example shows how to define a relationship for nodecellar (a :code:`SOURCE` node) to connect to mongodb (a :code:`TARGET`
node) as in Figure 1.

* We will define :code:`capabilities` on the mongodb node containing all information for nodecellar node to setup a connection.
* We will define the :code:`relationship` interfaces to setup the connection.

.. figure:: /_static/images/tosca-tutorial/nodecella_mongodb.png

  Figure 1. A topology with nodecellar connects to mongodb on a compute node

Steps
=====

Step 1. Define an endpoint capability in the TARGET node (mongodb)
------------------------------------------------------------------

An endpoint capability contains information of a :code:`TARGET` node for a :code:`SOURCE` node to setup the connection.

In the mongodb component, we define the following :code:`capabilities` block:

.. code-block:: yaml

  node_types:
    otc.nodes.SoftwareComponent.MongoDB:
      ...
      capabilities:
        mongo_db:
          type: tosca.capabilities.Endpoint.Database

In this example, we defined a new capability :code:`mongo_db` from the TOSCA type :code:`tosca.capabilities.Endpoint.Database`. The capability :code:`mongo_db` will inherit all default properties from the TOSCA type (e.g., :code:`port`, :code:`protocol`, :code:`url_path`) and show in the editor as in Figure 2.

In the editor, users can specifiy values for the capability :code:`mongo_db`. For example, they may set the :code:`port` to :code:`27017`.

.. figure:: /_static/images/tosca-tutorial/database_capability.png

  Figure 2. Capability

.. tip::

  The :code:`tosca.capabilities.Endpoint` also has a runtime attribute :code:`ip_address` (not shown in the Figure). The orchestrator will automatically set the IP address of the hosted compute node to this attribute. A :code:`SOURCE` node can use this runtime attribute to setup a connection.

Step 2. Define a requirement in the SOURCE node (nodecellar)
------------------------------------------------------------

In the nodecellar node, add the following :code:`requirements` block:

.. code-block:: yaml

  node_types:
    otc.nodes.WebApplication.Nodecellar:
      derived_from: tosca.nodes.WebApplication
      ...
      requirements:
        - mongo_db:
            # nodecellar requires a node that has the capability Endpoint.Database
            capability: tosca.capabilities.Endpoint.Database
            # nodecellar uses this relationship to setup the connection (see step 3)
            relationship: otc.relationships.NodejsConnectToMongo
            # (Optional) specifiy relationship instance one-to-one
            # it means, one nodecellar has one mongodb
            occurrences: [1, 1]

Step 3: Define the relationship
-------------------------------

Define a new relationship :code:`otc.relationships.NodejsConnectToMongo`, how nodecellar setups the connection with mongodb:

.. code-block:: yaml

  relationship_types:
    otc.relationships.NodejsConnectToMongo:
      derived_from: tosca.relationships.ConnectsTo
      interfaces:
        Configure:
          pre_configure_source:
            inputs:
              # The input DB_IP gets the runtime attribute ip_address of mongodb
              DB_IP: { get_attribute: [TARGET, mongo_db, ip_address] }
              # The input DB_PORT gets the mongodb port property
              DB_PORT: { get_property: [TARGET, port] }
              # The input NODECELLAR_PORT gets the nodecellar port property
              NODECELLAR_PORT: {get_property: [SOURCE, port]}
            implementation: scripts/set-mongo-url.sh

In the above example, we defined the interface :code:`pre_configure_source` by providing a shell script (e.g., :code:`set-mongo-url.sh`). The script uses the input parameters :code:`DB_IP`, :code:`DB_PORT`, :code:`NODECELLAR_PORT` to configure nodecellar to connect to mongodb.

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