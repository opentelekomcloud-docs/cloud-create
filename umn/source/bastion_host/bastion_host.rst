****************************
How to define a bastion host
****************************

1. Connect all computes to the admin network (step 1).
2. Drop the **SSHBastionHost** component on one compute (step 2). The given compute is now the bastion host.
3. Also connect the bastion host to the **public** network.

.. figure:: /_static/images/4-BastionHost-AdminNetwork-SSHBastionHost.png
  :width: 800

  Figure 1. Define a compute node as a bastion host

.. note:: If you forgot to connect the bastion host to the admin network, the designer will automatically connect it to the admin network for you.

Expected result
===============

1. During the deployment, the orchestration engine creates the security group :code:`Secgroup_BastionHost`  on the bastion host, which allows :code:`TCP` incoming traffic on port :code:`22` from the remote IP of the orchestration engine. As a result, it can connect to the bastion host over the public network (step 1).
2. The orchestration engine uses the bastion host as a jump host to SSH to the private :code:`Compute_2` in the admin network and deploy the service catalog :code:`HelloWorld` (step 2). To access :code:`Compute_2` over the bastion host, the orchestrator creates the security group :code:`Secgroup_Admin` on :code:`Compute_2`, which allows incoming traffic from the remote :code:`Secgroup_BastionHost` on port :code:`22`. It also enables :code:`TCP forwarding` on the bastion host.
3. After the deployment completes, the orchestration engine deletes the security group rule on the bastion host to prevent any further access to the bastion host.

.. figure:: /_static/images/4-BastionHost-Full-Flow.png
  :width: 800

  Figure 2. The deployment flows

.. note::

  **Auto-select Bastion Host**: If users do not define a bastion host explicitly, the designer will auto-select a compute node connecting to the public network as the bastion host. It also warns the users, which compute node is chosen as the bastion host before the deployment:

  .. figure:: /_static/images/5-BastionHost-auto-add-message.png
    :width: 800

    Figure 2. A warning message that a bastion host is auto selected before the deployment