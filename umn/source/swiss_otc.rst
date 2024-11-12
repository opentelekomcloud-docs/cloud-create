.. _Swiss OTC:

************************
Swiss Open Telekom Cloud
************************

This section summarizes the differences between two locations Swiss Open Telekom Cloud (Swiss OTC) vs. Open Telekom Cloud (OTC) and how you **design a template that works for both locations**.

1. Flavors
==========

Swiss OTC has a different set of flavors than OTC. At the time of writing, it only has **s3 flavors** (e.g., :code:`s3.medium.1`, :code:`s3.medium.2`).

In the following example, the Compute has 1CPU and 1GB Ram. We do not need to set a specific flavor for the Compute in the template.

.. figure:: /_static/images/swiss-otc-01.png
  :width: 800

  Figure 1. A Compute has 1 CPU and 1GB Ram

When you deploy the template on Swiss OTC, Cloud Create finds a matching flavor on Swiss OTC that has 1CPU and 1GB Ram (e.g., :code:`s3.medium.1`).

.. figure:: /_static/images/swiss-otc-02.png
  :width: 800

  Figure 2. Compute is updated with flavor s3.medium.1.

2. Availability Zones
=====================

OTC and Swiss OTC have different availability zones:

  +-----+------------------------------+------------------+
  |     | OTC                          | Swiss OTC        |
  +=====+==============================+==================+
  | AZs | eu-de-01, eu-de-02, eu-de-03 | eu-ch2a, eu-ch2b |
  +-----+------------------------------+------------------+

In the Design (Figure 1), you can set the **availability_zone** to :code:`az-01`, :code:`az-02`, or :code:`az-03`.

When you deploy the template in a location, Cloud Create finds the availability zones in the given location and updates the compute accordingly:

+-----------+----------+----------+--------------------------------------+
| AZ        | az-01    | az-02    | az-03                                |
+===========+==========+==========+======================================+
| OTC       | eu-de-01 | eu-de-02 | eu-de-03                             |
+-----------+----------+----------+--------------------------------------+
| Swiss OTC | eu-ch2a  | eu-ch2b  | eu-ch2b (Swiss OTC has only two AZs) |
+-----------+----------+----------+--------------------------------------+

.. figure:: /_static/images/swiss-otc-03.png
  :width: 800

  Figure 2. Compute is updated with AZ eu-ch2a.

.. note::

  You can set the **availability_zone** explicitly to :code:`eu-ch2a` or :code:`eu-ch2b` in the design. In such a case, Cloud Create keeps your setting.

3. EVS
======

Swiss OTC supports only SAS and SSD (`See documentation <https://docs.sc.otc.t-systems.com/elastic-volume-service/api-ref/openstack_cinder_apis_recommended/evs_disk/creating_evs_disks.html>`_). Therefore, you can set either :code:`SAS` or :code:`SSD` in the :code:`volume_type` of a **BootVolume** or **BlockStorage**.

.. figure:: /_static/images/swiss-otc-04.png
  :width: 800

  Figure 2. Set volume_type to SSD.

4. DNS
======

Swiss OTC supports only DNS private zone (no DNS public zone). See also :ref:`DNS`.

5. NATGateway
=============

OTC and Swiss OTC have different **spec** of NAT Gateway.

  +------+------------------------------------------+---------------------------------------------------------+
  |      | OTC                                      | Swiss OTC                                               |
  +======+==========================================+=========================================================+
  | Spec | Micro, Small, Medium, Large, Extra-large | Small, Medium, Large, Extra-large (has no spec 'Micro') |
  +------+------------------------------------------+---------------------------------------------------------+

For Swiss OTC, you can set the spec to :code:`Small`, :code:`Medium`, :code:`Large`, or :code:`Extra-large`.

.. figure:: /_static/images/swiss-otc-05.png
  :width: 800

  Figure 2. Set spec to 'Small'.

In the Deploy Setup, Cloud Create also checks for error if you set a value that is not supported by Swiss OTC.

.. figure:: /_static/images/swiss-otc-06.png
  :width: 800

  Figure 2. Error when set spec to 'Micro' on Swiss OTC.