****************************************************************************
How to specify a flavor, image, keypair, and availability zone for a compute
****************************************************************************

1. How to specify a flavor & image
==================================

1. In deploy **Setup**, click on **Configure cloud provider**.
2. Choose a matching flavor and image for a compute (e.g., :code:`Standard_Ubuntu_18.04_latest | s2.medium.1`).

.. figure:: /_static/images/2-compute-deploy-matching.png
  :width: 800

  Figure 1. The matching phase proposes a matched flavor and image of OTC

When we designed our :code:`HelloWorld` application, we specified CPU, RAM, and the operating system for the compute component. However, we did not specify a flavor and image. In the above Figure, the designer shows all available flavors and images that Open Telekom Cloud offers. For examples:

* If we input :code:`1CPU`, :code:`1GB RAM`, and the operating system :code:`ubuntu` for the compute, the designer shows the matching flavor :code:`s2.medium.1` and available ubuntu versions: :code:`18.04`, :code:`20.04`, and :code:`22.04`.

* If we input :code:`3GB RAM`, the designer will show the matching flavor :code:`s2.medium.4` with :code:`4GB RAM` (because Open Telekom Cloud does not offer a flavor with 3GB RAM).

2. How to specify a keypair & availability zone
===============================================

1. Select a **Compute**, the popup shows the following optional properties to configure:
2. **availability_zone**: Choose an availability zone for the compute (e.g., :code:`eu-de-01`).
3. **key_pair**: Click on the drop box to show a list of key pairs of the current user. If no key pairs are shown, go to the `console of Open Telekom Cloud <https://console.otc.t-systems.com/ecm/#/login>`_ and create one. Then go back to refresh the keypair list.

.. figure:: /_static/images/2-compute-deploy-matching-keypair.png
  :width: 800

  Figure 2. Choose a key pair for a compute