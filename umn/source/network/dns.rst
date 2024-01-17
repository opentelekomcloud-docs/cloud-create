*******************
How to create a DNS
*******************

.. _DNS:

You can create a Domain Name Service so that users can access your compute via the domain name instead of a floating IP address.

1. Steps
========

1. Connect a **Compute** to the **Public** network.
2. Put the component **DNSEndpoint** on the Compute.
3. Input :code:`myexample.com` in the **dns_name** field of the DNSEndpoint.

.. figure:: /_static/images/service-catalogs/dns1.png
  :width: 800

  Figure 1. DNS Endpoint

2. Expect result
================

1. One DNS public zone :code:`myexample.com.` with 2 record sets type A :code:`myexample.com.` and :code:`www.myexample.com.`  will be created on Open Telekom Cloud.

2. The record sets point to the floating IP of the public compute, which hosts the DNSEndpoint component (e.g., :code:`80.158.62.203`).

.. figure:: /_static/images/service-catalogs/dns2.png
  :width: 800

  Figure 2. DNS record sets are created on Open Telekom Cloud

3. Test
=======

1. Update your domain name at the registration service to the nameservers of Open Telekom Cloud: :code:`ns1.open-telekom-cloud.com` and :code:`ns2.open-telekom-cloud.com`.
2. Test your domain is resolved to the floating IP:

.. code-block:: bash

  $ dig myexample.com
  ...

  ;; ANSWER SECTION:
  myexample.com.    300 IN  A 80.158.62.203

  ;; AUTHORITY SECTION:
  myexample.com..   172800  IN  NS  ns1.open-telekom-cloud.com.
  myexample.com..   172800  IN  NS  ns2.open-telekom-cloud.com.

  ;; ADDITIONAL SECTION:
  ns2.open-telekom-cloud.com. 300 IN  A 93.188.242.252
  ns1.open-telekom-cloud.com. 300 IN  A 80.158.48.19

.. note::

  * The DNS zone takes effect only after you update the nameservers of your domain at the domain registrar to: :code:`ns1.open-telekom-cloud.com` and :code:`ns2.open-telekom-cloud.com`
  * This feature is not yet supported for Google Cloud. `Contact us <https://open-telekom-cloud.com/de/kontakt>`_ for feature request.