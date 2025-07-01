.. _Release notes:

*************
Release notes
*************

v2.18
=====

Cloud Container Engine template
-------------------------------

Launch Production-Ready Cloud Container Engine (CCE) Clusters in One Click with Cloud Create.

Say goodbye to complex setup scripts and hours of infrastructure toil. With Cloud Create, you can deploy a fully operational CCE cluster—complete with a NAT gateway, secure bastion host, and pre-configured :code:`kubectl` client—in just one click.

Your :code:`kubectl` client is ready to go right from the bastion host, giving you immediate access to manage and configure your cluster directly from our intuitive visual designer. No manual setup. No waiting.

Whether you're testing a new idea or deploying a production-grade environment, Cloud Create gets you there faster—securely and seamlessly.

Read more: :ref:`cce`.

.. figure:: /_static/images/service-catalogs/cce1.png
  :width: 900

  Figure 1. The CCE template

v2.17
=====

Private template
----------------

You can now design your app and save it as a private template. The private template will be available for all users of the same project to reuse. This function is useful if you want to design a premade template and share it with your team or organization. Read more: :ref:`template`.

.. figure:: /_static/images/private_template1.png
  :width: 700

  Figure 2. Save as template

v2.16
=====

Deployment in Netherlands
-------------------------

You can now deploy all templates in Netherlands. After login, switch to a project in the region Netherlands.

.. figure:: /_static/images/features/v2.16-netherlands.png
  :width: 800

  Figure 3. Switch to a project in Netherlands

Improve deploy setup messages
-----------------------------

The deploy setup logs display messages in 3 categories: error, warning, and info. We also validate the deployment for you and display any errors, warnings, or information you should know in each category, respectively.

.. figure:: /_static/images/features/v2.16-setup-logs.png
  :width: 800

  Figure 4. New deploy setup logs

v2.15
=====

Swiss Open Telekom Cloud
------------------------

Users can now login with IAM accounts from Swiss Open Telekom Cloud as well. When you login with an IAM account from Open Telekom Cloud (or Swiss Open Telekom Cloud) and deploy a template, the template will be deployed on Open Telekom Cloud (or Swiss Open Telekom Cloud), respectively.

.. figure:: /_static/images/features/v2.15-login-swiss-otc.png
  :width: 800

  Figure 5. Login with domain name from Swiss Open Telekom Cloud.

Read more about :ref:`Swiss OTC`.

OpenShift v4.16 release
-----------------------

OpenShift v4.16 has been released in the template. This is the latest stable version of OpenShift.

Read more about :ref:`openshift`.

Ansible v10.5.0
---------------

Starting from this version :code:`2.15.x`, we support Ansible :code:`10.5.0` (i.e., Ansible core :code:`2.17.4`), which is compatible for `target VM with Python 3.7 - 3.12 <https://docs.ansible.com/ansible/latest/reference_appendices/release_and_maintenance.html>`_ installed. The following table shows an example that our :code:`AnsibleTasks` component works with target OS RHEL 9 but not RHEL 8.

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

v2.14
=====

Gallery template
----------------

The gallery template enables users to create app from public templates. They can quickly deploy a template or design from a template.

.. figure:: /_static/images/features/overview-templates.png
  :width: 800

  Figure 6. OpenShift template

v2.13
=====

OpenShift template
------------------

Users can create a `Self-managed OpenShift Container Platform <https://www.redhat.com/en/technologies/cloud-computing/openshift/container-platform>`_ on Open Telekom Cloud from the :ref:`OpenShift`.

.. figure:: /_static/images/features/openshift.png
  :width: 800

  Figure 7. OpenShift template

v2.12
=====

History
-------

Users can view deployment logs in the **History** so they can audit all actions in the past.

.. figure:: /_static/images/features/deployment_history.png
  :width: 800

  Figure 8. Deployment history shows output of the Bash script 'HelloWorld' executed on a compute.