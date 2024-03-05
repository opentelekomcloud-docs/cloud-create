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
   google/index
   service_catalogs/index
   tosca_tutorials/index
   secrets
   git
   orchestration_workflow
   releases

1. Overview
===========

Cloud Create provides cloud developers a development and management platform to create applications on multi-cloud fast. In step 1, cloud architects design and develop the application for a tenant A. In step 2, an administrator of the tenant A deploys the application on Open Telekom Cloud, Swiss Cloud, and or Google Cloud.

.. figure:: /_static/images/features/overview.png
  :width: 900

  Figure 1. Overview

.. note::
  * Cloud architects and administrators can be the same user.
  * Deployment on Swiss Open Telekom Cloud is an upcoming feature.

1.1. How cloud architects design the application
------------------------------------------------

Cloud architects can design the application either from scratch or from an **application template**. To design the application, they drag and drop the components together like lego bricks as in the following example:

.. figure:: /_static/images/features/overview-design.png
  :width: 800

  Figure 2. An application topology with network, compute, ansible, bash scripts, and Grafana component.

* In the above example, (public/private) network and compute are **infrastructure components**. AnsibleTasks, Bash, and Grafana are **service components**.
* By using the Ansible and Bash components, cloud architects can write code to install softwares on a compute directly. Grafana is a ready-to-use deployable software solution.
* All components are provided by Open Telekom Cloud upon feature requests. In addition, users can define service components in TOSCA (i.e., a YAML file format) and import them to the designer. This import feature enables a **dynamic integration** with any software solutions (e.g., Grafana) and external SaaS with Open Telekom Cloud. More details on Section :ref:`How to define and import and a new service`.

.. note::
   * All service components are `opensource and available on our Github <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs>`_.

1.2. How administrators deploy the application
----------------------------------------------

Before the deployment can start, administrators select a **Version**, provide deployment **Inputs**, which were designed by the cloud architects, and select a cloud provider (e.g., Open Telekom Cloud) to deploy. In addition, the workflow to deploy the components are auto-generated and ready for administrators to review.

.. figure:: /_static/images/features/overview-deploy.png
  :width: 800

  Figure 3. Administrators select Open Telekom Cloud (OTC) to deploy

During the deployment, users can interactively click on a workflow step and see **Terraform** is applied for the infrastructure components. For the service components, the deployment script of the service (e.g., Ansible) is applied on the target compute.

.. figure:: /_static/images/features/deployment_logs.png
  :width: 800

  Figure 4. An example Terraform is generated and applied for a Compute

2. Why Cloud Create?
====================

The following table shows the differences between the web console and the marketplaces of any cloud providers:

.. figure:: /_static/images/features/features_compare.png
  :width: 800

  Figure 5. Features comparison

Both the web console, marketplace, and Cloud Create can deploy one cloud service. However, an application nowadays consists of multiple cloud services but not just one. Furthermore, each application has individual needs to adapt to the cloud differently. The web console and marketplace can bring up one service up and running separately but cannot automate an application with multiple services.

With Cloud Create, after creating an application from a template, you can modify the application to fit your individual needs. For example, one of our customer used Cloud Create to create their application from the OpenShift template, then they added a worker node with the GPU capability and wrote an Ansible script to deploy packages on the bastion host, etc. The given example shows that the modification can happen in every corner, from the cloud infrastructure to the software layer of your application. Finally, you can run update between versions on operation day 2 as well. To update between versions, Cloud Create auto-calculates the differences between the two versions and auto-generates the update workflow steps from one version to other one.

In short, Cloud Create manages the whole life cycle of an application: from design, adaptation, to deploy, and update.

3. New features
===============

OpenShift template (v2.13)
--------------------------

Users can create a `Self-managed OpenShift Container Platform <https://www.redhat.com/en/technologies/cloud-computing/openshift/container-platform>`_ on Open Telekom Cloud from the :ref:`OpenShift`.

.. figure:: /_static/images/features/openshift.png
  :width: 800

  Figure 6. OpenShift template

4. FAQ
======

4.1. What are the differences between Cloud Create and the web console
----------------------------------------------------------------------

With the `web console <https://console.otc.t-systems.com>`_, users can only create the cloud **infrastructure** manually. It means, they can create a network, a storage, a VM separately but without automation.

On the other hand, Cloud Create enables developers to design and automate the deployment of the whole application, which includes the cloud infrastructure and the **services** (i.e., not just the cloud infrastructure). In addition, developers can design the application in various versions and run update between them. Finally, Cloud Create provides application templates (e.g., OpenShift) to re-use and extend.

4.2. How can I login in to Cloud Create
---------------------------------------

You can log in to Cloud Create using an IAM user account with the **Tenant Administrator** role. This is the same credentials when you log in to the web console, **not the ICU account**.

If you do not have a user account in the IAM, see :ref:`How to login`.

4.3. Which components are supported for Open Telekom Cloud and Google Cloud
---------------------------------------------------------------------------

An overview of all supported components is available in Section :ref:`Components overview`.

4.4. Is Cloud Create opensource
-------------------------------

Cloud Create is based on two opensource projects Application Lifecycle Enablement for Cloud (Alien4cloud) and Ystia Orchestrator (Yorc). At Open Telekom Cloud, we further integrate it with OpenStack and Google Cloud, provide an easy-to-use UI, added features (e.g., secrets management, deployment update, OpenShift template, etc.), and enforce the strictest Privacy and Security Assessment (PSA) process of Deutsche Telekom.

All application templates and service components are opensource and available on `our Github <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs>`_. You can create pull requests to add more components and fix bugs.

4.5. Which Terraform version do you use
---------------------------------------

We use Terraform 1.5.4 under the Mozilla Public License v2.0 (MPL 2.0).