************
Ansible role
************

Step 1. Provide ansible role in as a zip file
=============================================

* In your local machine, create a folder :code:`tasks` with one file :code:`main.yml`.

.. code-block:: yaml

  - debug:
      msg:
        - "Hello! I am an {{ MY_NAME }}"
        - "I am running on {{ HOST }}"

* Zip the :code:`tasks` folder as :code:`configure.zip` and upload.

.. figure:: /_static/images/8-Ansible-role-zip.png
  :width: 800

  Figure 1. Zip the content of the ansible role

Step 2. Upload
==============

* Upload the :code:`configure.zip` as the artifact **configure** of the AnsibleRole.

.. figure:: /_static/images/8-Ansible-role-configure.png
  :width: 800

  Figure 2. Upload ansible role

Step 3. Provide ansible variables
=================================

In addition to the :code:`env` property, users can define ansible variables by providing the artifact **ansible_variables**:

* Define ansible variables in a yml file (e.g., :code:`myvar.yml`)

.. code-block:: yaml

  MY_NAME: example

* Upload :code:`myvar.yml` in the artifact **ansible_variables**.

Expected result
===============

During the deployment, the orchestrator extracts the zip file on the ansible controller, and applies the role on the target compute.

.. code-block:: bash

  Hello! I am an example
  I am running on Compute