.. _Release notes:

*************
Release notes
*************

v2.15 - Swiss Open Telekom Cloud
================================

1. Swiss Open Telekom Cloud
---------------------------

Users can now login with IAM accounts from Swiss Open Telekom Cloud as well. When you login with an IAM account from Open Telekom Cloud (or Swiss Open Telekom Cloud) and deploy a template, the template will be deployed on Open Telekom Cloud (or Swiss Open Telekom Cloud), respectively.

.. figure:: /_static/images/features/v2.15-login-swiss-otc.png
  :width: 800

  Figure 1. Login with domain name from Swiss Open Telekom Cloud.

Read more about :ref:`Swiss OTC`.

2. OpenShift v4.16 release
--------------------------

OpenShift v4.16 has been released in the template. This is the latest stable version of OpenShift.

Read more about :ref:`openshift`.

3. Ansible v10.5.0
------------------

Starting from this version :code:`2.15.x`, we support Ansible :code:`10.5.0` (i.e., Ansible core :code:`2.17.4`), which is compatible for `target VM with Python 3.7 - 3.12 <https://docs.ansible.com/ansible/latest/reference_appendices/release_and_maintenance.html>`_ installed. For examples:

+--------------+----------------+-------------------------------------+
| Target OS    | Python version | Compatible with Ansible core 2.17.4 |
+==============+================+=====================================+
| Ubuntu 20.04 | 3.8            | yes                                 |
+--------------+----------------+-------------------------------------+
| Ubuntu 22.04 | 3.10           | yes                                 |
+--------------+----------------+-------------------------------------+
| Ubuntu 24.04 | 3.12           | yes                                 |
+--------------+----------------+-------------------------------------+
| RHEL 9       | 3.9            | yes                                 |
+--------------+----------------+-------------------------------------+
| RHEL 8       | 3.6            | no                                  |
+--------------+----------------+-------------------------------------+

v2.14 - Gallery template
========================

The gallery template enables users to create app from public templates. They can quickly deploy a template or design from a template.

.. figure:: /_static/images/features/overview-templates.png
  :width: 800

  Figure 2. OpenShift template

v2.13 - OpenShift template
==========================

Users can create a `Self-managed OpenShift Container Platform <https://www.redhat.com/en/technologies/cloud-computing/openshift/container-platform>`_ on Open Telekom Cloud from the :ref:`OpenShift`.

.. figure:: /_static/images/features/openshift.png
  :width: 800

  Figure 3. OpenShift template

v2.12 - History
===============

Users can view deployment logs in the **History** so they can audit all actions in the past.

.. figure:: /_static/images/features/deployment_history.png
  :width: 800

  Figure 4. Deployment history shows output of the Bash script 'HelloWorld' executed on a compute.