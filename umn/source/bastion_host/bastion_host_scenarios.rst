**************************
Usecases of a bastion host
**************************

1. When do we need a bastion host?
==================================

The topology has a service catalog (e.g., :code:`HelloWorld`) in a private compute (e.g., :code:`Compute_2`).

.. figure:: /_static/images/4-NoBastionHost-3.png
  :width: 800

  Figure 1. Service catalog in private Compute_2

In this example, users did not define a bastion host explicitly. The orchestrator uses the public compute node (e.g., :code:`Compute`) as the bastion host by default to access the private :code:`Compute_2:code:`. The orchestrator does not need to access the private :code:`Compute_3` because it has no service catalog.

2. When we do NOT need a bastion host?
======================================

2.1 No service catalogs
-----------------------

The topology has only infrastructure components (e.g., network, compute, storage) and has no service catalogs.

.. figure:: /_static/images/4-NoBastionHost.png
  :width: 800

  Figure 2. No service catalogs at all

In this example, we neither need a bastion host nor an admin network. The orchestrator does not need to access any computes. Also, it does not check for SSH connection after the computes start.

2.2 No service catalogs in a private compute
--------------------------------------------

The topology has no service catalogs in a private compute (e.g., :code:`Compute_2`). It may have a service catalog (e.g., :code:`HelloWorld`) on a public compute (e.g., :code:`Compute`).

.. figure:: /_static/images/4-NoBastionHost-2.png
  :width: 800

  Figure 3. No service catalogs in the private Compute_2

In this example, the orchestrator checks for SSH connection after the public :code:`Compute` starts and accesses it **directly** over the internet to deploy the service catalog. The orchestrator does not need to access the private :code:`Compute_2` because this compute has no service catalog.