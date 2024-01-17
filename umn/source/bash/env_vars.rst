.. _bash env vars:

*************************************
Environment variables for Bash script
*************************************

1. Predefined environment variables
===================================

A Bash component has some properties, which will be available in the **configure** script as the environment variables:

* **COMPONENT_VERSION**: to input a version number (e.g., :code:`1.0`).

* **PROTOCOL**: to input a protocol (e.g., :code:`tcp`).

* **PORT**: to input a port (e.g., :code:`27017`).

* **NODE**: set to the node name of the component (e.g., :code:`Bash_1`).

* **HOST**: set to the name of the compute node, which the Bash component is hosted on (e.g., :code:`Compute_1`).

* **ip_address**: set to the runtime IP address of the compute node, which hosts this component.

2. User-defined environment variables
=====================================

Additionally, users can define custom environment variables by using the **env** property.

.. figure:: /_static/images/7-Bash-properties-env.png
  :width: 800

  Figure 1. Set environment variables for Bash

The **env** property is a map of string values. In this example, we add an entry with key :code:`FOO` and value :code:`bar`:

.. figure:: /_static/images/7-Bash-env.png
  :width: 800

  Figure 1. An example of emviroment FOO

Then we can access the variable :code:`FOO` in the **configure** script of the Bash component as follows:

.. code-block:: bash

  # Result: Hello bar
  echo "Hello $FOO"