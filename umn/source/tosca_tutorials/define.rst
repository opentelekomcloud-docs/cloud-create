.. _tosca define service catalog:

*******************************
How to define a service catalog
*******************************

The following tutorial shows how to model the service catalog :code:`HelloWorld` from the example. You can import your service catalog to Cloud Create, or exchange it with other users. You will learn how to control it (i.e., to :code:`create`, :code:`configure`, :code:`start`, :code:`stop`, and :code:`delete`) on a hosted compute node.

.. figure:: /_static/images/tosca-tutorial/tosca-helloworld.png

  Figure 1. HelloWorld example

1. Steps
========

Step 1. Import TOSCA types
--------------------------

* Create a file :code:`types.yaml`
* To import the TOSCA type definition:

.. code-block:: yaml

  imports:
    - tosca-normative-types:1.0.0-ALIEN20

Step 2. Define a node type
--------------------------

Define a new node type (e.g., :code:`myservice.nodes.SoftwareComponent.Python`) by deriving from the existing TOSCA type :code:`tosca.nodes.SoftwareComponent`:

.. code-block:: yaml

  node_types:
    # This is the node name, can be arbitrary
    myservice.nodes.SoftwareComponent.Python:
      derived_from: tosca.nodes.SoftwareComponent
      # optional icon
      tags:
        icon: /images/python.png

.. important:: A node type starting with :code:`otc.*` is not allowed. We reserve this domain for all types defined by the Open Telekom Cloud.

By deriving from the :code:`tosca.nodes.SoftwareComponent`, your component can be hosted on a compute node as follows:

.. figure:: /_static/images/tosca-tutorial/1_python.png
  :width: 800

  Figure 2. Python can be hosted on a compute node

The TOSCA type :code:`tosca.nodes.SoftwareComponent` also comes up with the default property :code:`component_version`. Therefore, the Python
component also inherits this property (see Figure 1, on the right). In the editor, users can specify the value for the property.

Step 3. Define custom node properties
-------------------------------------

You can add additional :code:`properties` for the Python component (e.g., add a new property :code:`hello_message` from the type :code:`string`). The new property is set to the default value :code:`Hello World!`. The :code:`required` field indicates that users have to specify a value for this property in the editor.

.. code-block:: yaml

  myservice.nodes.SoftwareComponent.Python:
    ...
    properties:
      hello_message:
        description: A simple message to print
        type: string
        required: true
        default: "Hello World!"

.. seealso::

  TOSCA also supports the following simple types for a property:
  * YAML Types: **integer**, **string**, **boolean**, **float**
  * TOSCA types:

    * **version**: (e.g., :code:`2.0.1`)
    * **range** (e.g., :code:`[0, 100]`, :code:`[ 0, UNBOUNDED ]`)
    * **list** (e.g., :code:`[ 80, 8080 ]`)
    * **map** (e.g., :code:`{ "firstname": "darth", "lastname": "vader" }`

Step 4. Define the interfaces
-----------------------------

The interfaces are the implementations to control the lifecycle of the component. The orchestration engine will call the interfaces at runtime to :code:`create`, :code:`configure`, :code:`start`, :code:`stop`, and :code:`delete` the component.

In the following example, we implement the :code:`start` interface by providing a python script :code:`start.py`. The orchestrator will call this a python script to start the :code:`HelloWorld` component.

.. code-block:: yaml

  myservice.nodes.SoftwareComponent.Python:
    ...
    properties:
      hello_message:
    ...
    interfaces:
      Standard:
        start:
          inputs:
            # the keyword SELF indicates the node itself.
            msg: {get_property: [SELF, hello_message]}
          implementation: scripts/start.py

The start interface has the input variable :code:`msp`. It gets the value from the node property :code:`hello_message`.

.. note::

  * Use the function :code:`get_property` to get the value of a node property.
  * Use the keyword :code:`SELF` to reference to the current node.

The input param :code:`msp:code:` of the :code:`start` interface is available as an environment variable in the python script, so you can use as follows:

.. code-block:: python

  # scripts/start.py
  print(msg)

.. tip:: We also support the following implementation scripts: python, shell script, and ansible.

Step 5. Define the output value
-------------------------------

We can define the output result for the Python component. The outputs can be displayed in the designer after the deployment completes, or it can be used as an input value for another components or another interfaces.

* Define an interface :code:`create` with a python script (e.g., :code:`create.py`):

.. code-block:: yaml

  interfaces:
    Standard:
      ...
      create:
        implementation: scripts/create.py

* Define an **environment variable** (e.g., :code:`myVar1`) in the python script:

.. code-block:: python

  # scripts/create.py

  from os import environ

  # Set value for the enviroment variable myVar1
  myVar1="Resolved {0}".format(var1)

* Define a runtime **attribute** (e.g., :code:`resolvedOutput1`), which gets the environment variable :code:`myVar1` from the interface :code:`create`:

.. code-block:: yaml

  myservice.nodes.SoftwareComponent.Python:
    ...
    attributes:
      resolvedOutput1: { get_operation_output: [SELF, Standard, create, myVar1]}

.. note::

  * Use the keyword :code:`attributes` to define the output result of a node.
  * Use the function :code:`get_operation_output` to get an output from an interface implementation.

Step 6. Package
---------------

* To package the TOSCA definition, zip all contents (the yaml definition file and the python script).
* Upload the zip file to the service catalog in **Catalog** / **Manage archives** / **Browse**.

.. figure:: /_static/images/tosca-tutorial/tosca-package.png
  :width: 800

  Figure 3. Upload TOSCA component

2. Expected result
==================

* In the editor, the Python component has the properties :code:`hello_message` (defined in step 2) and the attribute :code:`resolvedOutput1` (defined in step 5).
* During the deployment, the orchestrator executes the python scripts :code:`create.py` and :code:`start.py` on the hosted compute.

.. figure:: /_static/images/tosca-tutorial/2_python.png
  :width: 800

  Figure 3. ?

3. Links
========

* See `how this service catalog is modelled in TOSCA format <https://github.com/opentelekomcloud-blueprints/tosca-tutorials/blob/master/examples/python/types.yaml>`_.