*********************************
How to define a custom capability
*********************************

In Section :ref:`tosca relationship`, we used the default capability :code:`tosca.capabilities.Endpoint.Database`. The capability has the some default properties: :code:`port`, :code:`protocol`.

.. figure:: /_static/images/tosca-tutorial/database_capability.png

  Figure 1. Default properties of an endpoint capability

The following section shows how to define custom capabilities.

Steps
=====

Step 1. Define a custom capability
----------------------------------

To add more properties to a capability, or to set a default value to a capability, we define a custom one as follows:

.. code-block:: yaml

  capability_types:
    # define a custom capability
    tosca.capabilities.Endpoint.Database.Mongo:
      derived_from: tosca.capabilities.Endpoint.Database
      properties:
        port:
          type: integer
          default: 27017

Here, the custom capability :code:`tosca.capabilities.Endpoint.Database.Mongo` inherits all properties from the parent
type :code:`tosca.capabilities.Endpoint.Database` and set the :code:`port` property to the default value :code:`27017`.

Step 2. Update the target node to have the new capability
---------------------------------------------------------

.. code-block:: yaml

  node_types:
    otc.nodes.SoftwareComponent.MongoDB:
      ...
      capabilities:
        mongo_db:
          type: tosca.capabilities.Endpoint.Database.Mongo

Step 3. Update the source node to require a target node with the new capability
-------------------------------------------------------------------------------

.. code-block:: yaml

  node_types:
    otc.nodes.WebApplication.Nodecellar:
      derived_from: tosca.nodes.WebApplication
      ...
      requirements:
        - mongo_db:
            # nodecellar requires to a node that has the capability Endpoint.Database.Mongo
            capability: tosca.capabilities.Endpoint.Database.Mongo
          ...