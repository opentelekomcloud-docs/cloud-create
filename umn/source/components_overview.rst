.. _Components overview:

*******************
Components overview
*******************

The following table shows an overview of all components that you can use to design your application. Some components are not supported in Google Cloud but can be delivered within the given days (2 weeks is one release cycle of the product). If you have any requests, please `contact us <https://open-telekom-cloud.com/de/kontakt>`_.

Network
=======

.. note:: Cloud Gateway is not yet supported in Google Cloud so a private compute cannot access the Internet. It means you cannot install packages from a public repository for a private compute.

======================  ===========================================================================================  ====================  ==============
Components              Scenarios                                                                                    Open Telekom Cloud    Google Cloud
======================  ===========================================================================================  ====================  ==============
Public network          :ref:`Connect a compute to a public network <public network>`                                Yes                   Yes
Public network          Reuse an existing floating IP and assign to a compute                                        Yes                   No (2w)
Private network         :ref:`Create a private network with a given CIDR <Private network CIDR>`                     Yes                   Yes
Private network         :ref:`Add a private network with an existing network_id <Private network network_id>`        Yes                   No (2w)
Subnets and regions     :ref:`google static subnets`                                                                 No                    Yes
Security groups         :ref:`Create security group rules manually<Security group manual>`                           Yes                   Yes
Security groups         :ref:`Auto-generated security group rules <Security group auto generated>`                   Yes                   Yes
SNAT / Cloud Gateway    VMs with no floating ip can make outgoing requests to the Internet                           Yes                   No (2w)
DNS                     :ref:`Auto-generated DNS record set type A pointing to floating IP of network port <DNS>`    Yes                   No
DNS                     :ref:`Auto-generated DNS record set type A pointing to floating IP of network port <DNS>`    Yes                   No
VirtualIP               Create a Virtual IP Address and bind to multiple network ports as allowed addresses pair     Yes                   No
======================  ===========================================================================================  ====================  ==============

Storage
=======

================  =====================================================================================================  ====================  ==============
Components        Scenarios                                                                                              Open Telekom Cloud    Google Cloud
================  =====================================================================================================  ====================  ==============
Block storage     :ref:`Create a block storage with given size <EVS>`                                                    Yes                   Yes
Block storage     :ref:`Mount and format a block storage <EVS>`                                                          Yes                   Yes
Boot Volume       Create a boot volume for a VM using image id or image name                                             Yes                   No
Object storage    :ref:`Create an OBS bucket with auto-generated access key and specify users to upload objects<obs>`    Yes                   No (3w)
================  =====================================================================================================  ====================  ==============

PaaS
====

=============================  ==========================================  ====================  ==============
Components                     Scenarios                                   Open Telekom Cloud    Google Cloud
=============================  ==========================================  ====================  ==============
Relational Database Service    :ref:`Create a RDS with databases <rds>`    Yes                   No
=============================  ==========================================  ====================  ==============

Service components
==================

=======================  =========================================================================================================================================
Components               Scenarios
=======================  =========================================================================================================================================
Prometheus monitoring    :ref:`Create a monitoring system to collect metrics from VMs and send alerts <Prometheus>`
Hardening                :ref:`Hardening the OS and SSH on a VM to compliant with Privacy and Security Assessment process of Deutsche Telekom Group <Hardening>`
Nextcloud                Create the Nextcloud app with different template versions
Apache                   Deploy an Apache Webserver. on a VM and configure its vhost
MySQLServer              Deploy MySQL server on a VM, create multiple database
=======================  =========================================================================================================================================
