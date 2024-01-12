*************
Nextcloud app
*************

1. How to use
=============

1.1 Create app
--------------

You can create the `Nextcloud application <https://nextcloud.com/>`_ from the :code:`Nextcloud` template:

.. figure:: /_static/images/service-catalogs/nextcloud.png
  :width: 800

  Figure 1. Nextcloud template

Template v1.0.0
^^^^^^^^^^^^^^^

Nextcloud is deployed in one VM:

.. figure:: /_static/images/service-catalogs/nextcloud1.png
  :width: 800

  Figure 2. Nextcloud template v1

Template v2.0.0
^^^^^^^^^^^^^^^

Nextcloud is deployed in two VM:
* Apache Webserver, PHP, and Nextcloud app are deployed on the front-end :code:`Compute`.
* MySQL server and database is deployed on the back-end :code:`Compute_2`.

.. figure:: /_static/images/service-catalogs/nextcloud2.png
  :width: 800

  Figure 3. Nextcloud template v2

Template v3.0.0
^^^^^^^^^^^^^^^

Nextcloud app uses the Open Telekom Cloud service :ref:`rds` offered by Open Telekom Cloud as the MySQL Server.

.. figure:: /_static/images/service-catalogs/nextcloud2v3.png
  :width: 800

  Figure 4. Nextcloud template v3

Template v4.0.0
^^^^^^^^^^^^^^^

Nextcloud app uses the Open Telekom Cloud services :ref:`obs` and :ref:`rds` as the storage back-end and the MySQL Server, respectively.

.. figure:: /_static/images/service-catalogs/nextcloud2v4.png
  :width: 800

  Figure 5. Nextcloud template v4

.. important::

  * When a user Bob deploys the application, this template auto-creates an access key for Bob and uses this access key to create the OBS bucket. The auto-created access key is auto-deleted when Bob deletes the application (i.e., :code:`auto_create_access_key` is set by default).
  * Nextcloud app is configured to use the auto-created access key of Bob to upload objects in the OBS bucket.
  * When users undeploy the application, all objects in the OBS bucket are deleted (i.e., :code:`force_destroy` is set to true by default).
  * See :ref:`obs` for more details.

1.2. (Optional) Configure Nextcloud app
---------------------------------------

* **download_url**: Provide the URL to download nextcloud. Defaults to :code:`https://download.nextcloud.com/server/releases/latest.tar.bz2`.
* **datadir**: Specify the location to store Nextcloud data. Default to: :code:`/mnt/nc-data`.
* **password**: Specify admin password for Nextcloud. The password is auto-generated if not specified.

.. figure:: /_static/images/service-catalogs/nextcloud3.png
  :width: 800

  Figure 6. Nextcloud app config

1.2. (Optional) Configure the DNS
---------------------------------

* Input your domain in the **dns_name** field of Apache (e.g., :code:`myexample.com`).

.. figure:: /_static/images/service-catalogs/nextcloud3b.png
  :width: 800

  Figure 7. Configure DNS

2. Expected result
==================

1. The deployment outputs the floating IP and admin account:

.. figure:: /_static/images/service-catalogs/nextcloud4.png
  :width: 800

  Figure 8. Nextcloud app outputs

2. Access nextcloud using the floating IP:

.. figure:: /_static/images/service-catalogs/nextcloud5.png
  :width: 800

  Figure 9. Nextcloud app access

3. Access nextcloud using DNS:

If :code:`dns_name` is specified, one DNS public zone :code:`myexample.com.` with 2 record sets type A :code:`myexample.com.` and :code:`www.myexample.com.`  will be created on Open Telekom Cloud. The record sets point to the floating IP (e.g., :code:`80.158.45.177`):

.. figure:: /_static/images/service-catalogs/nextcloud4b.png
  :width: 800

  Figure 10. DNS created

.. note::
  The DNS zone takes effect only after you update the nameservers of your domain at the domain registrar to:
  :code:`ns1.open-telekom-cloud.com` and :code:`ns2.open-telekom-cloud.com`.


3. Links
========

* See `how Nextcloud web application is modelled in TOSCA format <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs/blob/main/nextcloud/types.yml>`_.