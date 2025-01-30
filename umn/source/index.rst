==================
Quick introduction
==================

.. toctree::
   :maxdepth: 1
   :hidden:

   register
   getting_started/index
   components_overview
   network/index
   storage/index
   bastion_host/index
   bash/index
   ansible/index
   security_group/index
   swiss_otc
   google/index
   service_catalogs/index
   tosca_tutorials/index
   secrets
   git
   orchestration_workflow
   releases



.. attention::

  Cloud Create has supported Swiss Open Telekom Cloud and OpenShift v4.16. See :ref:`Release notes`.

1. Overview
===========

Cloud Create is a **free-to-use** Cloud Management Platform to create applications from **premade templates** on Open Telekom Cloud and Swiss Open Telekom Cloud **fast**.

.. figure:: /_static/images/features/overview1.png
  :width: 700

  Figure 1. Overview

2. How
======

Users can:

1. Create an app from a premade template (e.g., the :ref:`openshift`).
2. Visually design and customize the template to fit their needs.
3. Deploy and update the application on Open Telekom Cloud.
4. Save a design as a private template for personal use or share it public with other users.

.. figure:: /_static/images/features/overview.png
  :width: 900

  Figure 2. Features

.. note::
  * The function "Save & Share templates" are upcoming features.

2.1. How cloud architects design the application
------------------------------------------------

Cloud architects can design the application from scratch or from premade templates. First, they select a template:

.. figure:: /_static/images/features/overview-templates.png
  :width: 800

  Figure 3. Select an app template to start.

Then they choose to "quick deploy" or start a new design from the template:

.. figure:: /_static/images/features/overview-templates2.png
  :width: 800

  Figure 4. Quick deploy OpenShift or Design using this template.

Cloud Create also comes up with a visual designer for less-coding or no-coding. Developers can drag and drop the components together like lego bricks as in the following example:

.. figure:: /_static/images/features/overview-design.png
  :width: 800

  Figure 5. An example with network, compute, ansible, bash scripts, and Grafana component.

* In the above example, the network and compute are **infrastructure** components. AnsibleTasks, Bash, and Grafana are **service** components.
* By using the Ansible and Bash components, developers can write code to execute on a compute directly.
* Grafana is an example of a service component. Developers can define new services and import them to the designer as well. More details on Section :ref:`How to define and import a new service`.

.. note::
   * App templates and service components are `opensource and available on our Github <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs>`_.

2.2. How administrators deploy the application
----------------------------------------------

In the **Deploy** Section, administrators can choose which **Version** to deploy, provide **Inputs**, and select a cloud provider (e.g., Open Telekom Cloud) to deploy. In addition, they can review the workflow before it starts.

.. figure:: /_static/images/features/overview-deploy.png
  :width: 800

  Figure 6. Administrators select Open Telekom Cloud to deploy

During the deployment, administrators can interactively click on a workflow step and see **Terraform** is applied for the infrastructure components. For the service components, they can see Ansible is applied on the target compute.

.. figure:: /_static/images/features/deployment_logs.png
  :width: 800

  Figure 7. An example Terraform is generated and applied for a Compute

3. Why Cloud Create?
====================

3.1. Resource Formation Service vs. Cloud Create
------------------------------------------------

The following table shows the differences between Resource Formation Service (RFS) and Cloud Create:

+-------------------------------------------------+----------------------------+--------------+
| Features                                        | Resource Formation Service | Cloud Create |
+=================================================+============================+==============+
| A template solution                             | yes                        | yes          |
+-------------------------------------------------+----------------------------+--------------+
| Support infrastructure stack                    | yes                        | yes          |
+-------------------------------------------------+----------------------------+--------------+
| Support service stack (Ansible, Bash, software) | no                         | yes          |
+-------------------------------------------------+----------------------------+--------------+
| Support stack upgrade                           | no                         | yes          |
+-------------------------------------------------+----------------------------+--------------+

a. Template solution
^^^^^^^^^^^^^^^^^^^^

Both RFS and Cloud Create are a template solution. In particular, RFS is a template solution for the infrastructure stack (e.g., Network, Storage, CCE, etc.).

Cloud Create is a template solution for both infrastructure stack and service stack (i.e., a full stack solution).

It means, Cloud Create can deploy OpenShift and many more external cloud solutions on top of Open Telekom Cloud. RFS cannot deploy OpenShift.

b. Stack upgrade
^^^^^^^^^^^^^^^^

RFS does not support stack upgrade. It means, after you deploy a stack, you cannot change the deployment.

Cloud Create supports stack upgrade. Both the infrastructure and service stack in the template can be changed after the deployment (e.g., users can scale up and down the OpenShift worker nodes or run Ansible scripts on a compute after the deployment).


3.2. Web console vs. Cloud Create
---------------------------------

The following table shows the differences between the Web console and Cloud Create:

.. figure:: /_static/images/features/features_compare.png
  :width: 800

  Figure 8. Features comparison

Both the Web console and Cloud Create can deploy one cloud service. However, an application nowadays consists of multiple cloud services but not just one. The Web console can bring up one service up and running separately but cannot automate an application with multiple services.

Cloud Create automates all cloud services in the template. In addition, you can modify the template to fit your individual needs (e.g., write an Ansible script to deploy additional packages, etc). Finally, you can run update between versions as well. To update between versions, Cloud Create auto-calculates the differences between the two versions and auto-generates the update workflow steps.

4. FAQ
======

4.1. How can I login in to Cloud Create
---------------------------------------

You can log in to Cloud Create using an **IAM user account** with the **Tenant Administrator** role. This is the the account created in the Identity Management of the Web console. If you do not have an IAM account, see Section :ref:`How to create an IAM user account`.

If you have an **ICU account** or you login from **Telekom MMS IdP via an SSO**, you can first login to the Web console and then create a new IAM account in the Web console. With the IAM account, you can login to Cloud Create.

4.2. Which components are supported
-----------------------------------

An overview of all supported components is available in Section :ref:`Components overview`.

4.3. Is Cloud Create opensource
-------------------------------

Cloud Create is based on two opensource projects Application Lifecycle Enablement for Cloud (Alien4cloud) and Ystia Orchestrator (Yorc). At Open Telekom Cloud, we further integrate it with OpenStack and Google Cloud, provide an easy-to-use UI, added features (e.g., secrets management, deployment update, the :ref:`openshift`, etc.), and enforce the strictest Privacy and Security Assessment (PSA) process of Deutsche Telekom.

All premade templates and service components are opensource and available on `our Github <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs>`_. You can create pull requests to add more components and fix bugs.

4.4. Which Terraform version do you use
---------------------------------------

We use Terraform 1.5.4 under the Mozilla Public License v2.0 (MPL 2.0).

4.5. Which Ansible version do you support
-----------------------------------------

Before version :code:`2.15.x`, we supported Ansible :code:`2.10.7`.

Starting from version :code:`2.15.x`, we support Ansible :code:`10.5.0` (i.e., Ansible core :code:`2.17.4`), which is compatible with `target VM having Python 3.7 - 3.12 <https://docs.ansible.com/ansible/latest/reference_appendices/release_and_maintenance.html>`_ installed. The following table shows an example that our :code:`AnsibleTasks` component works with target OS RHEL 9 but not RHEL 8.

+-----------------------------+----------------+-------------------------------------+
| Target OS                   | Python version | Compatible with Ansible core 2.17.4 |
+=============================+================+=====================================+
| Ubuntu 20.04                | 3.8            | yes                                 |
+-----------------------------+----------------+-------------------------------------+
| Ubuntu 22.04                | 3.10           | yes                                 |
+-----------------------------+----------------+-------------------------------------+
| Ubuntu 24.04                | 3.12           | yes                                 |
+-----------------------------+----------------+-------------------------------------+
| RHEL 9                      | 3.9            | yes                                 |
+-----------------------------+----------------+-------------------------------------+
| RHEL 8                      | 3.6            | no                                  |
+-----------------------------+----------------+-------------------------------------+
| Standard_openSUSE-Leap_15.6 | 3.6            | no                                  |
+-----------------------------+----------------+-------------------------------------+
| Enterprise_SLES_15.6        | 3.6            | no                                  |
+-----------------------------+----------------+-------------------------------------+

5. Webinar
==========

.. image:: /_static/images/features/webinar.png
    :width: 700
    :alt: Webinar OpenShift meets Open Telekom Cloud | T-Systems
    :target: https://www.youtube.com/watch?v=tHcKIE-U2s8

In this Webinar, you will learn:

* OpenShift license and support.
* Differences between Kubernetes/CCE vs. OpenShift.
* Demo how to deploy OpenShift.
* Demo insights of the OpenShift console and its key features.