*******************************************
How to define the security group explicitly
*******************************************

.. _Security group manual:

You can define security groups for a Port as follows:

* Drop a **SecurityGroup** to the editor.
* Connect on the **dependency** of a Port to the **feature** of the **SecurityGroup**.
* Drop one or more **SecurityGroupRule** on the **SecurityGroup**.

.. figure:: /_static/images/7-Bash-secgroup-explicit.png
  :width: 800

  Figure 1. Define security group explicitly

* Specify the security group rule (e.g., allow :code:`inbound` traffic with protocol :code:`tcp` on port :code:`8080` from the remote :code:`0.0.0.0/0`).

.. figure:: /_static/images/7-Bash-secgroup-rule-explicit.png
  :width: 800

  Figure 2. An example of security group rule