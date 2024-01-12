.. _mysql:

************
MySQL server
************

1. About
========

Use this component to deploy a MySQL server and databases.

2. How to use
=============

2.1. How to deploy a MySQL server?
----------------------------------

1. Drop the **MySQLServer** component on a Compute.
2. (Optional) Specify the **root_password**. If the password is not specified, it is auto-generated.

.. figure:: /_static/images/service-catalogs/mysql1.png
  :width: 800

  Figure 1. MySQL server

2.2. How to create MySQL database and user?
-------------------------------------------

1. Drop more than one **MySQLDatabase** components on the **MySQLServer**.
2. Specify the database **name**, **encoding**, **user** and **password** accordingly.

.. figure:: /_static/images/service-catalogs/mysql2.png
  :width: 800

  Figure 2. MySQL database

Expect result
^^^^^^^^^^^^^

* By default, MySQL server is installed at: :code:`/var/lib/mysql`.

2.3. How to move the default datadir to a location on a Block Storage?
----------------------------------------------------------------------

1. Drop the **BlockStorage** and **LinuxFileSystem** components on the Compute.
2. Click **LinuxFileSystem** and specify the mount point in the **location** field (e.g., :code:`/mnt`).
3. Connect **config_filesystem_as_datadir** (on the right) of the **MySQLServer** to the feature point (on the left) of the **LinuxFileSystem**.

Expect result
^^^^^^^^^^^^^

* The Block Storage is mount to the VM at: :code:`/mnt`.
* MySQL server is installed in the new datadir at: :code:`/mnt/mysql`.
* AppArmor (for Debian) and SELinux (for Redhat) are configured to allow access to the new location.

2.4 How to define a connection to MySQL database using TOSCA?
-------------------------------------------------------------

* Define a TOSCA ConnectsTo relationship. For example:

.. code-block:: yaml

  otc.relationships.NextcloudConnectsToDb:
    derived_from: tosca.relationships.ConnectsTo
    interfaces:
      Configure:
        description: Start installing nextcloud and connect to MySQL database
        post_configure_source:
          inputs:
            MYSQL_HOST: { get_attribute: [TARGET, database_endpoint, ip_address] }
            MYSQL_DB: { get_property: [TARGET, name] }
            MYSQL_USER: { get_property: [TARGET, user] }
            MYSQL_PASSWORD: { get_property: [TARGET, password] }
          implementation: playbooks/nextcloud-post-configure-source.yaml

2. Links
========

* See the template example in: Topology template / :code:`Nextcloud`.
* See `how this service catalog is modelled in TOSCA format <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs/blob/main/mysql/types.yml>`_.