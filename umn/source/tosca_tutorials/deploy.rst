*********************************************
How to deploy a file or a folder to a compute
*********************************************

1. How to
=========

1.1. How to deploy a file
-------------------------

During the deployment of a software component, sometimes we need to deploy an external file on the target compute node. In such a case, we can define an :code:`artifacts` in the node definition as below:

.. code-block:: yaml

  node_types:
    otc.nodes.SoftwareComponent.ComponentA:
      derived_from: tosca.nodes.SoftwareComponent
      interfaces:
        Standard:
          create: scripts/create.sh
      artifacts:
        # specify the artifact name
        - hello_script:
            type: tosca.artifacts.File
            # path to an existing file, relative path to this yaml definition
            file: scripts/helloworld.js

The orchestrator will send the file (available at the path :code:`scripts/helloworld.js`) to the compute node, on which the software component is deployed.

In the interface implementation, we can get the location of the artifact as follows:

.. code-block:: bash

  # scripts/create.sh
  # The artifact name "hello_script" resolves to the location of the file artifact
  # on the target compute node
  cat $hello_script

In the editor, you can also upload and overwrite the artifact :code:`hello_script` for a given topology:

.. figure:: /_static/images/tosca-tutorial/artifacts.png
  :width: 800

  Figure 1. Select Artifact

1.2. How to deploy a folder
---------------------------

The artifact can be a folder as well:

.. code-block:: yaml

  node_types:
      ...
      artifacts:
        # specify an artifact name
        - scripts:
            type: tosca.artifacts.File
            # "myscripts" is an existing folder, relative path to this yaml definition
            file: myscripts

In the interface implementation, we can get the path to the folder in the same way:

.. code-block:: bash

  # scripts/create.sh
  # The artifact name "scripts" resolves to the location of the folder on the target
  # compute node.
  ls $scripts

.. note::

  The orchestration engine is responsible for copying the artifact to the compute node and makes its path available as the environment variable (i.e., the artifact name) in the configuration script for you.

2. Links
========

* See `full example <https://github.com/opentelekomcloud-blueprints/tosca-tutorials/blob/master/examples/artifact/types.yml>`_.