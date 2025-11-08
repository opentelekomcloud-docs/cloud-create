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
   save_template
   secrets
   orchestration_workflow
   releases
   tutorial_videos


.. attention::

  **v2.19 released:** Argo CD template - Instant App Deployment Made Simple. Read more: the :ref:`argocd`.

1. Overview
===========

Cloud Create is a free-to-use **visual cloud management platform** that helps you **reduce cloud engineering footprint**, **accelerate time-to-market**, and deploy everything **from infrastructure to applications** in just one click.

Unlike traditional cloud platforms that require deep infrastructure knowledge and manual setup, Cloud Create offers a visual designer with **a growing premade templates** that enables you to deploy complex environments such as OpenShift and Argo CD in minutes. Whether you're working on Open Telekom Cloud or Swiss Open Telekom Cloud, Cloud Create empowers you to go from idea to running applications with speed and simplicity.

Cloud Create is built on open standards and powered by open-source TOSCA templates. Every deployment is transparent - **no hidden scripts, no black boxes**. You can inspect, customize, and extend each template directly from `our public GitHub repository <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs>`_ for **full transparency and reproducibility**.

.. figure:: /_static/images/features/overview1.png
  :width: 1000

  Figure 1. Overview

2. From Idea to Cloud in Just Four Steps
========================================

Cloud Create makes cloud-native development simple, fast, and collaborative. You can go from ideas to cloud in minutes:

**1. Start with a premade template**

Choose from premade templates like :ref:`OpenShift`, :ref:`cce`, or :ref:`argocd` - no setup headaches, just ready-to-go blueprints.

**2. Visually design and customize**

Use the drag-and-drop visual designer to tailor infrastructure, software components, and automation to your exact needs - without writing complex YAML or Terraform.

**3. Deploy and update**

Launch your app with a single click. Need to make changes later? Redeploy updates just as easily, directly from the visual interface.

**4. Save and share your work**

Turn your customized app into a private template for your team, or publish it publicly on Open Telekom Cloud so others can benefit from your expertise.

.. figure:: /_static/images/features/overview.png
  :width: 1000

  Figure 2. From Idea to Cloud in Just Four Steps

2.1. Start with a premade template
----------------------------------

With Cloud Create, deploying a ready-to-use solution is easy, starting from **premade templates** or building from scratch:

.. figure:: /_static/images/features/overview-templates.png
  :width: 800

  Figure 3. First select a premade template to start

2.2. Visually design and customize
----------------------------------

After selecting a premade template, you have the flexibility to **quickly deploy** apps right away - or use them as a starting point to **visually design and customize** your apps.

.. figure:: /_static/images/features/overview-templates2.png
  :width: 800

  Figure 4. Quick deploy or design using the template

In the design, you can seamlessly combine **infrastructure components** (e.g., compute, networking, storage), **platform components** (e.g., CCE, RDS), and **software components** (e.g., Bash, Ansible, Helm, Argo CD) all together. By combining them in one single design, you have an **end-to-end automation** solution.

.. figure:: /_static/images/features/overview-design2.png
  :width: 800

  Figure 5. Visual Design a full-stack application

2.3. Deploy and update
----------------------

When your design is ready, simply hit “Deploy” - and Cloud Create will provision everything for you on Open Telekom Cloud in just minutes. Deploying apps with Cloud Create is simple, controlled, and fully transparent:

**1. Select, Configure, and Launch**

In the **Deploy** section, you choose the desired **application version**, fill in required **inputs**, and select a **cloud provider** - like Open Telekom Cloud. Before launching, they can easily **review the deployment workflow** to ensure everything is ready.

.. figure:: /_static/images/features/overview-deploy.png
  :width: 800

  Figure 6. Administrators select Open Telekom Cloud to deploy

**2. Interactive, Step-by-Step Execution**

During deployment, you can **follow each step in real time**. Click on any stages to see exactly what's happening - **Terraform** automates infrastructure provisioning, while **Ansible** configures services on the target compute.

.. figure:: /_static/images/features/deployment_logs.png
  :width: 800

  Figure 7. An example Terraform is generated and applied for a Compute

2.4. Save and share your work
-----------------------------

2.4.1. Extend What’s Possible with Custom TOSCA Services
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

With Cloud Create, you're not limited to what's out of the box. Cloud engineers can define new services using the open TOSCA format and seamlessly import them into the visual designer.

Whether it's a custom VM setup, a specialized software stack - define it once in TOSCA, then drag-and-drop it like any built-in components.

.. note::
   * Read more about :ref:`How to define and import a new service`.
   * Our premade templates and service catalogs `are opensource and available on Github <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs>`_ for transparency and contributions.

2.4.2. Export Terraform & TOSCA
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

With Cloud Create, you can share your solution design as premade template, download it in TOSCA format, or export the cloud infrastructure in Terraform.

.. figure:: /_static/images/features/export.png
  :width: 800

  Figure 8. Export options

3. FAQ
======

3.1. How can I login in to Cloud Create
---------------------------------------

You can log in to Cloud Create using an **IAM user account** with the **Tenant Administrator** role assigned to a project. This is the the user account created in the Identity Management of the Web console. If you do not have an IAM user account, see Section :ref:`How to create an IAM user account`.

.. attention::

  If you have an **ICU account** or you login from **Telekom MMS IdP via an SSO**, you can first login to the Web console and then create a new IAM user account in the Web console. With the IAM user account, you can login to Cloud Create.

  If you have an IAM user account with the **Security Administrator** role (highest privilege), we strongly recommend you to create a new IAM user account with the **Tenant Administrator** role and assign it to a project (less privilege). Then you can use the new IAM user to login Cloud Create.

3.2. Which components are supported
-----------------------------------

An overview of all supported components is available in Section :ref:`Components overview`.

3.3. Is Cloud Create opensource
-------------------------------

Cloud Create is based on two opensource projects Application Lifecycle Enablement for Cloud (Alien4cloud) and Ystia Orchestrator (Yorc). At Open Telekom Cloud, we further integrate it with OpenStack and Google Cloud, provide an easy-to-use UI, added features (e.g., secrets management, deployment update, the :ref:`openshift`, etc.), and enforce the strictest Privacy and Security Assessment (PSA) process of Deutsche Telekom.

All premade templates and service catalogs are `opensource and available on Github <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs>`_ for transparency and contributions.

3.4. Which Terraform version do you use
---------------------------------------

We use Terraform 1.5.7 under the Mozilla Public License v2.0 (MPL 2.0).

3.5. Which Ansible version do you support
-----------------------------------------

We currently support Ansible :code:`10.5.0` (i.e., Ansible core :code:`2.17.4`), which is compatible with `target VM having Python 3.7 - 3.12 <https://docs.ansible.com/ansible/latest/reference_appendices/release_and_maintenance.html>`_ installed. The following table shows the compatibility matrix with Operating Systems:

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

3.6. How can I suggest a feature or ask a quick question
--------------------------------------------------------

Please `ask a question in the Open Telekom Cloud Community <https://community.open-telekom-cloud.com/community?id=community_ask_question>`_. Choose the forum **Orchestration and Automation** and select topic **Cloud Create** to ask a question.

.. figure:: /_static/images/ask-question.png
  :width: 800
  :target: https://community.open-telekom-cloud.com/community?id=community_ask_question