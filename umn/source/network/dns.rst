.. _DNS:

*******************
How to create a DNS
*******************

You can create a Domain Name Service so that users can access your compute via the domain name instead of a floating or private IP address.

1. How to create a public DNS
=============================

1. Connect a **Compute** to the **Public** network.
2. Click on the network port and set the **dns_name**.

.. figure:: /_static/images/service-catalogs/dns1.png
  :width: 800

  Figure 1. Compute in a public network

3. Input the **Domain** and **Subdomain** field (e.g., :code:`myexample.com` and :code:`www`, respectively)

.. figure:: /_static/images/service-catalogs/dns1b.png
  :width: 800

  Figure 2. Input

.. important::

  * The domain must be globally unique in the Domain Name Service of Open Telekom Cloud.

.. note::

  * Blank subdomain: Traffic will be routed to the domain :code:`myexample.com`.
  * Subdomain "www": Traffic will be routed to :code:`www.myexample.com`, which is usually used for a website.
  * Subdomain "*": Traffic will be routed to any subdomain of the domain.

Expect result
-------------

1. One DNS public zone :code:`myexample.com.` with one record set type A :code:`www.myexample.com.` pointing to the **floating IP** of the public compute (e.g., :code:`80.158.91.193`).

.. figure:: /_static/images/service-catalogs/dns2.png
  :width: 900

  Figure 2. Record set points to floating IP address

Test
----

1. Update your domain name at the registration service to the nameservers of Open Telekom Cloud: :code:`ns1.open-telekom-cloud.com` and :code:`ns2.open-telekom-cloud.com`. Or to test on localhost, update the nameservers of your localhost to the nameservers of Open Telekom Cloud.
2. Test your domain is resolved to the floating IP:

.. code-block:: bash

  $ dig www.myexample.com

  ;; QUESTION SECTION:
  ;www.myexample.com.   IN  A

  ;; ANSWER SECTION:
  www.myexample.com.  300 IN  A 80.158.91.193

.. note::

  * The DNS zone takes effect only after you update the nameservers of your domain at the domain registrar to: :code:`ns1.open-telekom-cloud.com` and :code:`ns2.open-telekom-cloud.com`

.. important::

  **Swiss Open Telekom Cloud** does not support DNS public zone, but only DNS private zone. When you set a dns_name to a network port, a DNS private zone will be created instead.

.. _DNS Private:

2. How to create a private DNS
==============================

1. Put the compute in a private network (i.e., the network port does not connect to a public network)
2. Click on the network port and set the **dns_name** (same as above).

.. figure:: /_static/images/service-catalogs/dns3.png
  :width: 800

  Figure 3. Compute in a private network

Expect result
-------------

* One DNS private zone :code:`myexample.com.` with one record set type A :code:`www.myexample.com.` pointing to the private IP address of the network port (e.g., :code:`10.0.0.147`)

.. figure:: /_static/images/service-catalogs/dns4.png
  :width: 900

  Figure 4. Record set points to private IP address