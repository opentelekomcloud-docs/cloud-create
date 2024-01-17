***********************************
How to auto-generate security group
***********************************

.. _Security group auto generated:

When two Bash components are in a connection relationship, the orchestration engine can auto-generate a security group to enable the traffic between them. For the auto generation of security groups, users specify the port property.

* On the component :code:`Bash_1`, specify the port :code:`8080`.
* On the component :code:`Bash_2`, specify the port :code:`27017`.

.. figure:: /_static/images/7-Bash-auto-secgroup.png
  :width: 800

  Figure 1. Specify the port property for auto-generated security group

**Expected result**:

* Because :code:`Compute_1` is a public compute, a security group is generated for the :code:`Compute_1`, which allows the traffic from the public network (i.e., allow the :code:`TCP` protocol from the remote IP :code:`0.0.0.0/0` on port :code:`8080`).
* Because :code:`Bash_1` connects to :code:`Bash_2` internally, a security group is generated for the :code:`Compute_2`, which allows the private traffic from the :code:`Compute_1` (i.e., allow the :code:`TCP` protocol from the remote security group of :code:`Compute_1` on port :code:`27017`).
* The Open Telekom Cloud console also shows the auto-generated security groups attached to the ports of the two compute nodes. The figure below shows an example that the security group is auto-generated for the :code:`Compute_2` accordingly.

.. figure:: /_static/images/7-Bash-auto-secgroup-otc.png
  :width: 800

  Figure 2. Example of auto-generated security group on the OTC console