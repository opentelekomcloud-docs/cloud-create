**********************************
How to save a design as a template
**********************************

You can save a design as a private or public template.

1. How to save a design as a private template
=============================================

1.1. Use case
-------------

* A cloud engineer designs an application and saves it as a template so that other users of the same project can re-design or quick deploy it.
* The cloud engineer in this case can be from the same organization or an external consultant service. In the later case, the domain administrator creates an IAM account for the cloud engineer to design the template. The oganization can quick deploy an app from the template later.

1.2. How to
-----------

* Go to **Save application** option / Click on **Save as template**.

.. figure:: /_static/images/private_template1.png
  :width: 700

  Figure 1. Save as template button

* Input information for your template: Template name (e.g., :code:`M̀y template`), version (:code:`0.1.0`), Designed by (any name you like), and description.

.. figure:: /_static/images/private_template2.png
  :width: 700

  Figure 2. Input template name "My template"

* The template :code:`My template` is now visible in the Section **Private templates**. All users in the same project can see this template.

.. figure:: /_static/images/private_template3.png
  :width: 700

  Figure 3. Private template Section

* Click on the template :code:`My template`. Now you can quick deploy, design using this template, or to delete it.

.. figure:: /_static/images/private_template4.png
  :width: 700

  Figure 4. Manage a template

1.3. Permissions
----------------

* A private template is visible to a project.
* IAM users with the :code:`Tenant Guest` role can view the template and **Design using this template**.
* IAM users with the :code:`Tenant Administrator` role can **Design using this template**, **Quick Deploy** and **Delete** the private template.

+----------------------+----------------------------+--------------+--------+
| IAM user role        | Design using this template | Quick Deploy | Delete |
+======================+============================+==============+========+
| Tenant Guest         | yes                        | no           | no     |
+----------------------+----------------------------+--------------+--------+
| Tenant Administrator | yes                        | yes          | yes    |
+----------------------+----------------------------+--------------+--------+

2. How to save a design as a public template
============================================

2.1. Use case
-------------

* A cloud engineer designs an application and saves it as a public template so that all users from all organization domains can re-design or quick deploy it.

2.2. How to
-----------

We will provide a button to export a private template as a public template later. For the moment you can create a pull request in our public repository as follows:

1. Design an application and click **Download topology**. The application topology will be downloaded as a zip file.

.. figure:: /_static/images/public-template.png
  :width: 700

  Figure 5. Manage template option

2. `Clone this repository <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs.git>`_
3. Create a new folder (e.g., :code:`my_template`) inside the :code:`templates` folder.
4. Exact the zip file to the folder :code:`my_template`.
5. Open the file :code:`topology.yml` and update the template with your information. For example:

.. code-block:: yaml

  metadata:
    template_name: My Template
    template_version: 0.1.0
    template_author: Dr. Vo
    # Optional link to a document how to use the template
    template_documentation: "https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs/blob/main/README.md"
    # Optional images you want to display in the slide, prefer 1920x1200
    template_images:
      - /images/thumbnail.jpg
      - /images/image1.png
      - /images/image2.png

  description: >
    This is my template description.

6. Push change to your clone repository.
7. Create a pull request to our repository so we can review your template and publish your template. It will be visible in the **Public templates** Section for everyone to quick deploy and to design using this template.
