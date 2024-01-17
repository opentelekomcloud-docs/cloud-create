.. _secrests:

******************************************
How to define secrets for your application
******************************************

1. About
========

You can set any properties of a service component as secrets. This is useful in case you do not want to expose sensitive data as plain text (e.g., a database password) in the topology description.

2. How to use
=============

Step 1. Set a property as secret
--------------------------------

1. Open the properties menu of any components (e.g., the :code:`root_password` property of the :code:`MySQLServer` component).
2. Select **Set as secret**.
3. Save the application.

.. figure:: /_static/images/secrets1.png
  :width: 700

  Figure 1. Set the root_password property as secret

Step 2. Input the secret value
------------------------------

1. Select the **Setting** tab.
2. The :code:`root_password` secret was created in Step 1 but has no value. Select it.
3. Input the value (e.g., :code:`Test1234`).

.. figure:: /_static/images/secrets2.png
  :width: 700

  Figure 2. Create the root_password secret

.. important:: Only users from Open Telekom Cloud with the :code:`Tenant Administrator` role in the same project has the permission to view and edit the secret value.

3. Expected result
==================

The topology description shows the :code:`root_password` property gets an input from the :code:`get_secret` function:

.. figure:: /_static/images/secrets3.png
  :width: 700

  Figure 3. Topology shows get_secret

When the application is deployed, the :code:`root_password` property will be resolved with the secret value :code:`Test1234`.

3. How secure is my secrets
===========================

* In step 2, the designer uses the authentication token of the user to encrypt the secret. During the deployment, the orchestration engine uses the user authentication token to decrypt the secret.
* It means, the system works on behalf of the user to encrypt and decrypt a given secret. Without the authentication token from a user with the :code:`Tenant Administrator` role in the same project, the system itself cannot decrypt the secrets. Therefore, our secret management system has a higher security in comparison to just encrypt the data with a symmetric key.