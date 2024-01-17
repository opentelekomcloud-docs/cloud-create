.. _Bastion host:

***********************
How to use Bastion host
***********************

A **bastion host** is a jump-host that the orchestration engine uses to access all computes in a private network to deploy a service catalog. An **admin network** is a private network connecting to all compute nodes. The orchestration engine uses the admin network for provisioning. The following tutorial shows how you can define a bastion host and an admin network in the topology.

.. figure:: /_static/images/4-BastionHost.png
  :width: 800

  Figure 1. An example how to use a bastion host

.. toctree::
  :maxdepth: 1
  :hidden:

  admin_network
  bastion_host
  bastion_host_scenarios