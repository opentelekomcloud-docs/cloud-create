.. _Fixed IP :

***************************************
How to specify a fixed IP for a compute
***************************************

* Click on the **Port** of the compute node.

* Type :code:`10.0.0.2` in the **ip_address** field (This address is within the IP range of the given network :code:`10.0.0.0/24`).

.. figure:: /_static/images/2-network-port-properties.png
  :width: 800

  Figure 1. Specify a fixed IP address for a Port

.. note::
  If no ip_address specified, an IP address will be auto-generated for the given compute within the IP range of the given private network.


The Port component has additional properties to configure:

* **is_default**: If the compute has more than one port, users can select this option to set the default gateway to the given Port. By default, the Port with access to the public network has the default gateway set.

* **ip_range_start**, **ip_range_end**: Currently not supported.
