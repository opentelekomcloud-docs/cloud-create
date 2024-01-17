*********************
How to deploy & debug
*********************

1. Deploy the app
=================

1. When the app is deployed, the **Review** tab shows which cloud resources are being created on Open Telekom Cloud. In this example, a virtual router, network, port, floating IP, and a Virtual Machine (VM) are created.

2. Click on a component (e.g., **Compute**) to show the deployment logs of it:

.. figure:: /_static/images/2-compute-deploy-review.png
  :width: 800

  Figure 1. Review shows Terraform configurations of Compute

3. In the above example, the logs show that the Terraform configuration for the Compute is generated and being applied.

4. In the console of Open Telekom Cloud, you can see the VM :code:`ctd-Compute-0` is created as well:

.. figure:: /_static/images/2-compute-deploy-review-2.png
  :width: 800

  Figure 2. OTC Console shows the new VM ctd-Compute-0

5. After the VM is created, the orchestration engine tests the SSH connection to the VM. After the test, the VM is ready to install a service catalog.

.. figure:: /_static/images/2-compute-deploy-result.png
  :width: 800

  Figure 3. Orchestration engine tests the SSH connection to the VM

6. The orchestration engine uses Ansible to zip and copy the :code:`HelloWorld` script to the VM (via SSH), and executes it on the VM. The script prints out the :code:`Hello World!` text in the deployment logs:

.. figure:: /_static/images/2-compute-deploy-result-helloworld.png
  :width: 800

  Figure 4. Orchestration engine deploys the HelloWorld python script via SSH

7. During the deployment, the orchestration engine creates a security group rule :code:`Secrule_inbound_BasionHost` that allows it to access the VM. After the deployment completes, the orchestration engine deletes this security group rule to prevent any further access to the VM:

.. figure:: /_static/images/2-compute-deploy-result-secrule-bastion-host.png
  :width: 800

  Figure 5. Orchestration engine deletes Secrule_inbound_BasionHost

.. seealso::
   The :ref:`Bastion host` Section will explain the deployment workflow in more details.

2. How to debug an error?
=========================

During the deployment if something goes wrong, you can:

1. Click on the error step.
2. Click on the **Errors** logs filter.

In the following example, the :code:`Vpc_helloWorld` failed to :code:`install` because a router in the same project exists with the same name.

.. figure:: /_static/images/debug-see-error.png
  :width: 800

  Figure 6. A VPC is failed to create

3. How to report a bug?
=======================

To report a bug to us, you can click the **download** button to export the deployment logs. Remember to remove any sensitive information from the deployment logs before sending it to us.