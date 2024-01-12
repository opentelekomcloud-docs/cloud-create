.. _rds:

***************************
Relational Database Service
***************************

1. About
========

* Use this component to deploy the Relational Database Service (RDS) on Open Telekom Cloud. With RDS, you can deploy a MySQL, PostgreSQL, or Microsoft SQL Server.
* RDS is a cloud service with auto-backup, auto-upgrade and high availability features, which are fully managed via Open Telekom Cloud console.
* Because the cloud service is fully managed by Open Telekom Cloud, users cannot SSH to the RDS Virtual Machine. If you wish to have a full control of the VM, use the :ref:`mysql` instead.

2. How to use
=============

2.1. How to deploy a MySQL server
---------------------------------

1. Drop the **RDSMySQL** component.
2. Connect **RDSMySQL** to a **Private** network. (Optional) connect it to a **Public** network if you wish to assign a floating IP to the RDS.
3. Specify the **component_version** (e.g., :code:`8.0`). For all supported versions, see `Help Documentation <https://docs.otc.t-systems.com/usermanual/rds/en-us_topic_0043898356.html>`_.
4. Specify the **root_password** (e.g., :code:`Test1234`). If you do not want to expose the password as plaintext, set it as a secret (Step 4b). See :ref:`secrests`.

.. figure:: /_static/images/service-catalogs/rds1.png
  :width: 800

  Figure 1. RDSMySQL

5. Specify the **instance_size** (e.g., :code:`1vCPU|2GB` for 1 vCPU and 2GB RAM).
6. Specify the **volume_type** for the Block Storage (e.g., :code:`SATA` or :code:`SSD`).
7. Specify the **volume_size** for the Block Storage (e.g., :code:`40` GB).
8. Specify the **backup_keep_days** for the backup retention time (e.g., :code:`7`). If set to :code:`0`, auto-backup feature is disabled.
9. Specify the **ha_mode** (e.g., :code:`single` for one instance).
10. Specify the **availability_zone** (e.g., :code:`eu-de-03`).

2.2. How to deploy the MySQL server in high availability mode
-------------------------------------------------------------

1. Specify the **ha_mode** as :code:`ha`.
2. Specify **two** **availability_zone** (e.g., :code:`eu-de-01` and :code:`eu-de-03`).
3. Specify **ha_replication_mode** (:code:`async` or :code:`semisync`).

2.3. How to get the IP address of RDS
-------------------------------------

1. Select **RDSMySQL** component.
2. Set the attribute **private_address** as output properties.

.. figure:: /_static/images/service-catalogs/rds3.png
  :width: 800

  Figure 2. Set output for RDS

The deployment will output the private IP of RDS. Alternatively, you can get the IP via the Open Telekom Cloud console.

2.4. How to create MySQL database and user
------------------------------------------

1. Drop more than one **MySQLDatabaseConnector** components on a **Compute**.
2. Connect **MySQLDatabaseConnector** to **RDSMySQL** via **connect_to_mysql_server**.
3. Specify the database **name**, **encoding**, **user** and **password** accordingly.

.. figure:: /_static/images/service-catalogs/rds2.png
  :width: 800

  Figure 3. Using MySQLDatabaseConnector

3. Expect result
================

* A MySQL Client (i.e., the :code:`PyMySQL` package) is deployed on the Compute VM.
* The orchestration engine uses the Compute VM to access the remote RDS on port :code:`3306` and create the given database and user on the remote RDS.

.. tip::

  If you do not wish to use the connector component, you can create the database and user manually using any MySQL client
  e.g., :code:`mysql -h <RDS_IP_ADDRESS> -P 3306 -u root -p`

