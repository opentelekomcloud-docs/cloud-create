**********************************
How the orchestration engine works
**********************************

The following section explains how the orchestration engine works in details.

1. OpenStack Heat vs. Cloud Create
==========================================

To understand the orchestration engine, we compare it with the :code:`OpenStack HEAT` orchestration engine as in Figure 1:

.. figure:: /_static/images/4-heat_vs_ctd.png
  :width: 800

  Figure 1. Cloud Create vs OpenStack Heat

1.1 Active connection is not required
-------------------------------------

The HEAT orchestrator (Figure 1, on the left) requires an agent pre-installed on the VMs. After the VMs start, the agent runs as a daemon process on the VMs. Both the agent and the HEAT orchestrator connect to the same messaging queue (i.e., RabbitMQ). The HEAT orchestrator pushes tasks to the queue. The agent pulls tasks from it and sets up the VMs accordingly. This orchestration type requires an active :code:`AMQP` connection all the time.

Unlike HEAT, our orchestrator does not require an agent pre-installed and running all the time on the VMs. Instead, it uses :code:`Ansible` to deploy a service catalog on the target VMs over a bastion host. Before the deployment, the orchestrator creates a security group on the bastion host that allows it to access the bastion host (i.e., allow :code:`TCP` incoming traffic on port :code:`22` from the remote IP of the orchestrator). After the deployment completes, this security group rule is auto deleted.

Figure 2 shows an example: At the beginning of the workflow, the orchestrator creates a security group on the bastion host (step 1), then enables :code:`TCP forwarding` on the bastion host (step 2), and auto-deletes the security group rule after the deployment completes (step 3).

.. figure:: /_static/images/4-BastionHost-Workflow.png
  :width: 800

  Figure 2. Deployment workflow

.. note:: In comparison to HEAT, Cloud Create does not require an active AMQP connection on the VMs all the time.


1.2 Fully isolated workflow
---------------------------

For each ansible execution in the workflow, the orchestrator starts a new **ansible controller container** and executes ansible from inside the container. After a workflow step completes or fails, the orchestrator terminates the container. As a result, a workflow execution from multiple tenants are fully isolated with each other.

.. figure:: /_static/images/4-workflow_ansible_docs.png
  :width: 800

  Figure 2. Orchestrator starts a sandbox container

1.3 Key Management System supported
-----------------------------------

When users deploy their applications, the orchestration engine uses the user OpenStack token to work on behalf of them and provision resources on Open Telekom Cloud. It means, the orchestration engine itself cannot make any changes to the tenant without the user authentication. This is similar to the :code:`OpenStack Heat` orchestration engine, which also uses the OpenStack token to provision resource on behalf of the authenticated users.

Recall that the orchestrator uses ansible to deploy service catalogs on the computes via SSH. For this purpose, the orchestrator auto-generates an SSH key for each deployment. The public part of the SSH key is installed on the VMs (via cloud-init). The private part is encrypted in the Key Management System using the user OpenStack token. Without the user OpenStack token, the orchestrator itself cannot decrypt this private key.

2. Error handling of the orchestration engine
=============================================

The orchestrator retries a workflow step when the following errors occur:

2.1. Terraform error
--------------------

* The orchestrator fails to apply terraform while creating (or deleting) an OpenStack compute, router, floating IP, network, port, security group, security group rule.
* Retry: 1 time.

2.2 REST APIs error
-------------------

The backend server is unreachable
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* The backend server of Open Telekom Cloud is temporarily not available and the REST API responses an HTTP error message :code:`The backend server is unreachable`.
* Retry: 5 times.

2.3 Error during VM bootstrap
-----------------------------

* The VM is created successfully, but the orchestrator cannot check the connection to the VM after 2 min.

* The deployment fails immediately. Users should login to the VM and check why cloud-init fails.

2.4 Ansible execution connection error
--------------------------------------

* Ansible is executed on a VM but terminated with exit code 4 (i.e., unreachable host).
* Retry: 1 time.

2.5. Workflow steps error
-------------------------

* When one step in the workflow fails, the orchestrator waits 2 minutes for the other parallel steps in the workflow to complete. If they do not terminate after 2 minutes, the orchestrator also sets them as failed.
