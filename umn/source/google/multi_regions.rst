.. _google static subnets:

***************************************************************
Design your application with static subnets in multiple regions
***************************************************************

The following tutorial describes how to design an application with one (global) private network spanning in two regions of Google Cloud. Each region has one subnet and one compute connecting to the subnet (e.g., :code:`Compute` connects to :code:`Subnet`, :code:`Compute_2` connects to :code:`Subnet_2`). The compute in the first region (e.g., :code:`Compute`) has access to the internet.

.. figure:: /_static/images/google/multi-regions.png
  :width: 800

  Figure 1. Multiple subnets example

Design
======

Step 1. Define a subnet for the network
---------------------------------------

* Drop the **Subnet** component on the **Private** network.

.. figure:: /_static/images/google/subnet-step1.png
  :width: 800

  Figure 2. Define a subnet

Step 2. Connect a compute to the subnet
---------------------------------------

* Click on the network point (on the right side of the **Compute**) and connect it to the connection point of the **Subnet**.

.. figure:: /_static/images/google/subnet-step2.png
  :width: 800

  Figure 3. Connect a compute to the subnet

Step 3. Define a CIDR range for the subnet
------------------------------------------

* Click on the **Subnet**.
* Type :code:`10.0.0.0/24` in the **cidr** field.

.. figure:: /_static/images/google/subnet-step3.png
  :width: 800

  Figure 3. Define CIDR

.. note:: The cidr field is mandatory for defining a subnet.

Step 4. Define a fixed ip address for the compute (optional)
------------------------------------------------------------

* Click on the **Port** of the compute.
* Type :code:`10.0.0.3` in the **ip_address** field. The ip_address :code:`10.0.0.3` is within the range of the network above (:code:`10.0.0.0/24`).

.. figure:: /_static/images/google/subnet-step4.png
  :width: 800

  Figure 4. Define fixed ip address

.. note:: If no ip_address specified, an ip address will be auto-generated within the ip range of the subnet during the deployment.

Step 5. Define the second compute and subnet
--------------------------------------------

* Drop another **Subnet** component on the Private network. Now we have two subnets: :code:`Subnet` and :code:`Subnet_2`.
* Click on the **Subnet_2**. Type :code:`10.0.1.0/24` in the **cidr** field.
* Drop another **Compute** component (e.g., :code:`Compute_2`) and connect it to **Subnet_2**.

.. figure:: /_static/images/google/subnet-step5.png
  :width: 800

  Figure 5. Define second subnet

Step 6. Connect the first compute to the public
-----------------------------------------------

* Drop a **Public** network.
* Connect the **Port** of :code:`Compute` to the link point (on the left side) of the **Public** network.

.. figure:: /_static/images/google/subnet-step6.png
  :width: 800

  Figure 6. Connect compute to public

Deploy
======

1. Go to **Deploy**.
2. Choose the **Google** provider.
3. In the **Configure cloud provider**:
4. Choose the **zone** for the computes in **different regions** (e.g., :code:`europe-north1-a` for :code:`Compute` and :code:`europe-west1-c` for :code:`Compute_2`).

.. figure:: /_static/images/google/subnet-step7.png
  :width: 800

  Figure 7. Choose zone europe-north1-a for Compute

In summary, we have two computes with two subnets in two different regions:

.. figure:: /_static/images/google/subnet-step7b.png
  :width: 800

  Figure 8. Two computes in two different regions europe-north1 and europe-west1

Expected result
===============

* The VPC :code:`private` network is created with two subnets (e.g., :code:`private-subnet` and :code:`private-subnet-2`) in two cidr ranges (:code:`10.0.0.0/24` and :code:`10.0.1.0/24`) and in two regions (e.g., :code:`europe-north1` and :code:`europe-west1`), respectively.

.. figure:: /_static/images/google/multi-regions-result2.png
  :width: 800

  Figure 9. Two subnets are created

* Two VMs will be created in the two separated subnets.
* :code:`Compute-0` has a fixed ip address :code:`10.0.0.3` (as specified in step 4) and has an external IP.

.. figure:: /_static/images/google/multi-regions-result1.png
  :width: 800

  Figure 10. Two computes are created