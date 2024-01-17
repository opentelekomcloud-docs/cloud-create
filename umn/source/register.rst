.. _How to login:

************************************
How to create a user account & login
************************************

1. How to login to Cloud Create
===============================

* Go to: https://designer.otc-service.com
* Login using an IAM user account with the **Tenant Administrator** role (*). This is the same credentials when you log in to the web console, **not the ICU account**.

.. figure:: /_static/images/register-login.png
  :width: 400

  Figure 1. Login to Cloud Create using domain name, username, and password

.. note:: (*) To design and deploy the application, login with a user account with the role **Tenant Administrator**. Alternatively, if you only need to design the application, login with a user account with the role **Tenant Guest**. This is particular useful when you want a cloud architect to design the application for you but do not allow him or her to deploy anything in your tenant.

You cannot login?
-----------------

* You may login with an **ICU account**. ICU account is not supported.
* You may have **Virtual MFA Device** enabled for login authentication. Please choose either SMS, Email, or disable it instead (Go to the console / click on username / My credentials / Login authentication).

2. How to choose a project
==========================

* After logging in, you can choose a **project**, where you want to design and deploy your applications:

.. figure:: /_static/images/register-choose-projects.png
  :width: 900

  Figure 2. Choose a project

.. _How to create an IAM user account:

3. How to create an IAM user account
====================================

In case you do not have an IAM user account, please ask your domain administrator to create one for you. The following tutorial shows how to create a new user and assign the given user to a project in the IAM:

2.1. Create a project (optional, if you don't have one)
-------------------------------------------------------

* Login to the `web console <https://console.otc.t-systems.com>`_.
* Go to: **Identity Management**

.. figure:: /_static/images/register-iam-button.png
  :width: 900

  Figure 2. Identity Management

1. Go to: **Projects**
2. Go to: **Create Project**.
3. Input project name: `eu-de_test`

.. figure:: /_static/images/register-create-project.png
  :width: 900

  Figure 3. Create a project

2.2. Create a user group for the project
----------------------------------------

1. Go to: **User Groups**.
2. Go to: **Create User Group**
3. Input a group name: `test`

.. figure:: /_static/images/register-create-group.png
  :width: 900

  Figure 4. Create a user group

2.3. Set permissions for the new user group
-------------------------------------------

1. Go to: **User Groups**
2. Search for the new group `test`.
3. Click **Authorize**.

.. figure:: /_static/images/register-modify-group.png
  :width: 900

  Figure 5. Authorize the group test

* In **Step 1. Select Policy/Role**, search for the role **Tenant Administrator** and select it.

.. figure:: /_static/images/register-modify-group2.png
  :width: 900

  Figure 6. Select role Tenant Administrator for the group

.. tip:: If you want a user to design the application for you but do not allow him or her to deploy anything in your project, select the role **Tenant Guest** instead.

* In **Step 2. Select Scope**, choose **Region-specific projects**.
* Search for the project `eu-de_test` and select it.

.. figure:: /_static/images/register-modify-group3.png
  :width: 900

  Figure 7. Select project eu-de_test for the group

Now users in the group `test` have the permissions to provision cloud resources in the project `eu-de_test`.

2.4. Create a new user in the user group
----------------------------------------

* Go to: **Users** / **Create User**.

.. figure:: /_static/images/register-create-user1.png
  :width: 900

  Figure 8. Input username 'testuser' and email address

* In **Step 2. Add User to Group**, select the new group `test`.

.. figure:: /_static/images/register-create-user2.png
  :width: 900

  Figure 8. Add user to group test

Now the new user `testuser` has the role `Tenant Administrator` to provision cloud resources in the project `eu-de_test` and can login to Cloud Create.