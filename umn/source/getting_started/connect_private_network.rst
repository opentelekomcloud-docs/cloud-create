*************************************
How to connect a compute to a network
*************************************

1. How to connect a compute to a private network
================================================

* Under **Infrastructure**, drop a **Compute** and a **Private** network component in the editor.

.. figure:: /_static/images/0-new-application-compute.png
  :width: 800

  Figure 1. Add a compute node to the topology

* Click on the **network** point (on the right side of the Compute) and connect it to the **connection** point of the Private network.

.. figure:: /_static/images/2-network-private.png
  :width: 800

  Figure 2. Connect a compute node to a network node

2. How to connect a compute to a public network
===============================================

.. _public network:

* Drop a **Public** network component to the editor.
* Click on the **link** point (on the right side) of the **Port** node and connect to the **link** point (on the left side) of the Public network.

.. figure:: /_static/images/2-network-public.png
  :width: 800

  Figure 3. Connect a Port to a public network