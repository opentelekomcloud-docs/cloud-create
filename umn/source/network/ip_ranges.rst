****************************************
How to create or reuse a private network
****************************************

1. How to create a new network
==============================

.. _Private network CIDR:

1. Create a new **Network** (e.g., :code:`Private`).
2. Input :code:`10.0.0.0/24` in the **cidr** field.

.. figure:: /_static/images/2-network-private-properties.png
  :width: 800

  Figure 1. Specify an IP range for a network

.. important::

  OTC allows the following network ranges:

  * 10.0.0.0/8-24
  * 172.16.0.0/12-24
  * 192.168.0.0/16-24

.. note::
    If a network has no CIDR specified, the designer will auto-set it with the next available CIDR, starting from :code:`10.0.0.0/24`, :code:`10.0.1.0/24`, :code:`10.0.2.0/24`, etc.

2. How to reuse an existing network
===================================

.. _Private network network_id:

Users can reuse an existing Network as follows:

1. Get the :code:`network_id` (e.g., 354b6773-f52c-4c95-8ec8-d7ce0a712bd9)

.. code-block:: bash

  $ openstack network list
  +--------------------------------------+--------------------------------------+--------------------------------------+
  | ID                                   | Name                                 | Subnets                              |
  +--------------------------------------+--------------------------------------+--------------------------------------+
  | 354b6773-f52c-4c95-8ec8-d7ce0a712bd9 | 9e702e5e-95bd-4ea0-b018-8fb0c7c117a9 | fedb8970-1a2b-4a21-b8e2-747b4e12ef6e |
  +--------------------------------------+--------------------------------------+--------------------------------------+

2. Create a network (e.g., :code:`Private_2`) and specify the :code:`network_id`:

.. figure:: /_static/images/2-network-private-properties-existing.png
  :width: 800

  Figure 2. Specify an existing network

3. Expect result
================

* The network :code:`Private` is created with the ip_range :code:`10.0.0.0/24` (as in step 1).
* The network :code:`Private_2` is reused. The compute is connected to both existing network and the new one.
* In our design, one topology has at most one VPC network. Therefore, the VPC of the network :code:`Private_2` is also reused to attach all subnets of :code:`Private` and :code:`Private_2`.

4. Auto network validation
==========================

When you save the topology, the designer will validate it for any errors. The example below shows an error that two networks in the topology having overlapping CIDRs:

.. figure:: /_static/images/2-network-private-cidr-error.png
  :width: 800

  Figure 3. Example of overlapped CIDR error