********************************************
Design your application with dynamic subnets
********************************************

The Section :ref:`google static subnets` shows how to define multiple subnets for a network **explicitly**. However, users may not want to define the subnets explicitly. In such a case, the Cloud Create will auto-generate one subnet for each region dynamically as follows:

Step 1. Design
==============

* Design an application with two computes connecting to the same :code:`Private` network. The Private network has no subnets.
* Click on the :code:`Private` network and specify the **cidr**: :code:`10.0.0.0/24`.
* Connect the first compute (e.g., :code:`Compute`) to the :code:`Public` network.

.. figure:: /_static/images/google/auto-subnets.png
  :width: 800

  Figure 1. Auto-generated subnet example

Step 2. Deploy
==============

Case 1. Two computes in the same region
---------------------------------------

1. Go to **Deploy** / **Google** / **Configure cloud provider**.
2. Choose the **zone** for the two computes in the **same region** :code:`europe-west1` (e.g., choose :code:`europe-west1-b` and :code:`europe-west1-c` for :code:`Compute` and :code:`Compute_2`, respectively).

Expected result
^^^^^^^^^^^^^^^

* Google Cloud requires one region has at least one subnet. The designer auto-generates one subnet :code:`Private_subnet_europe_west1` for the region :code:`europe-west1` in the :code:`Private` network.
* The subnet has the **same cidr** of the :code:`Private` network (i.e., :code:`10.0.0.0/24`).
* Both computes :code:`Compute` and :code:`Compute-2` connect to this subnet.

.. figure:: /_static/images/google/auto-subnets2.png
  :width: 800

  Figure 2. Private_subnet_europe_west1 is auto-generated for two computes in one region

* The :code:`routing_mode` of the :code:`Private` network is auto set to :code:`REGIONAL` (if not set) since all computes are in the same region.

.. figure:: /_static/images/google/auto-subnets-result1b.png
  :width: 800

  Figure 3. The routing_mode is auto set to REGIONAL

Case 2. Two computes in different regions
-----------------------------------------

1. Go to **Deploy** / **Google** / **Configure cloud provider**.
2. Choose the **zone** for the computes in **two different regions** (e.g., choose the zone :code:`europe-north1-a` and :code:`europe-west1-c` for :code:`Compute` and :code:`Compute_2`, respectively).

Expected result
^^^^^^^^^^^^^^^

* Google Cloud requires one region has at least one subnet. Because we have two regions, the designer auto-generates two subnets for the region :code:`europe-north1` and :code:`europe-west1`.
* The cidr of the :code:`Private` network (i.e., :code:`10.0.0.0/24`) is **auto subnetting** in two **equal ranges** for each subnet (i.e., :code:`10.0.0.0/25` and :code:`10.0.0.128/25`).
* Two computes connect to two separated subnets in different regions.

.. figure:: /_static/images/google/auto-subnets-result2.png
  :width: 800

  Figure 4. Two subnets are auto-generated for two regions

* The :code:`routing_mode` of the :code:`Private` network is auto set to :code:`GLOBAL` (if not set) so that the two computes from different regions can access each other via the internal IP address.

.. figure:: /_static/images/google/auto-subnets-result2b.png
  :width: 800

  Figure 5. The routing_mode is auto set to GLOBAL

.. tip::
  Auto-generated subnet is useful if you cannot decide the location of the computes at the design time (i.e., whether the computes are co-located in one or in different regions) but at the deployment time. In such a case, Cloud Create will transform the topology before the deployment for you.
