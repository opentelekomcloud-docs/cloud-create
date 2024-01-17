****************
Apache Webserver
****************

1. About
========

Use this component to deploy a Apache Webserver.

2. How to use
=============

2.1. How to deploy Apache Webserver
-----------------------------------

1. Drop the **Apache** component on a Compute.
2. Specify the **document_root**, where httpd will serve files. Defaults to :code:`/var/www/html`.

.. figure:: /_static/images/service-catalogs/apache1.png
  :width: 800

  Figure 1. Apache Webserver

Expect result
^^^^^^^^^^^^^

* After deployment, you can access the example :code:`index.html` via the floating IP. The security groups to allow incoming traffic on port 80 and 443 are auto created.

.. figure:: /_static/images/service-catalogs/apache2.png
  :width: 800

  Figure 2. Example index.html

2.2. How to enable SSL with self-signed certificates?
-----------------------------------------------------

* Select Apache / **ssl_enabled** / choose **selfsigned**.

Expect result
^^^^^^^^^^^^^

* Self-signed certificates are generated at: :code:`/etc/apache2/ssl`.
* Apache listens on port 443 and uses the self-signed certificates.
* Apache redirects HTTP to HTTPS.

2.3. How to generate Let's Encrypt certificates with a domain name?
-------------------------------------------------------------------

1. Connect the **Compute** to the **Public** network.
2. Select Apache / **ssl_enabled** / **letsencrypt**.

.. figure:: /_static/images/service-catalogs/apache3.png
  :width: 800

  Figure 3. Apache letsencrypt

3. Input :code:`myexample.com` in the **dns_name** field.
4. Configure the nameservers of your domain at the domain registrar to: :code:`ns1.open-telekom-cloud.com` and :code:`ns2.open-telekom-cloud.com`.

.. figure:: /_static/images/service-catalogs/apache4.png
  :width: 800

  Figure 4. Apache domain name

Expect result
^^^^^^^^^^^^^

1. One DNS public zone :code:`myexample.com.` with 2 record sets type A :code:`myexample.com.` and :code:`www.myexample.com.`  will be created on Open Telekom Cloud.
2. The record sets point to the floating IP of the public compute, which hosts the Apache Webserver (e.g., :code:`80.158.62.203`).

.. figure:: /_static/images/service-catalogs/dns2.png
  :width: 800

  Figure 5. DNS Endpoint

3. Let's encrypt is installed on the VM in stand-alone mode (before Apache is up and running). In the stand-alone mode, it brings up a webserver, and generates certificates for the given domain :code:`myexample.com` and subdomain :code:`www.myexample.com` and performs the :code:`HTTP-01` challenge on port 80 to verify the domain.
The certifcates are generated at :code:`/etc/letsencrypt/live/DNS_NAME`.

.. note:: The domain takes effect only after you update the nameservers of your domain at the domain registrar to: :code:`ns1.open-telekom-cloud.com` and :code:`ns2.open-telekom-cloud.com`. Otherwise, the :code:`HTTP-01` challenge for the given domain will fail.

2.4. How to deploy PHP with the Apache Webserver?
-------------------------------------------------

1. Drop the **PHP-FPM** on a Compute.
2. Connect **PHP-FPM** / **depend_on_webserver** to the **Apache** component.

.. figure:: /_static/images/service-catalogs/apache5.png
  :width: 800

  Figure 6. PHP

3. Configure Apache vhost with the following :code:`extra_parameters` to enable passthrough to PHP-FPM:

.. figure:: /_static/images/service-catalogs/apache6.png
  :width: 800

  Figure 7. Configure Apache vhost

.. code-block:: bash

  ProxyPassMatch ^/(.*\.php(/.*)?)$ "fcgi://127.0.0.1:9000/var/www/html"

3. Links
========

* See an example to deploy the Nextcloud application in: Topology template / :code:`Nextcloud`.
* See `how this service catalog is modelled in TOSCA format <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs/blob/main/apache/types.yml>`_.