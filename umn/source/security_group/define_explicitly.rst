*******************************************
How to define the security group explicitly
*******************************************

.. _Security group manual:

If users do not want the orchestration engine to auto-generate security groups for them, they can define the security groups explicitly on a Port as follows:
* Drop a **SecurityGroup** node to the editor.
* Connect on the **dependency** point of a Port to the **feature** point of the **SecurityGroup**.

.. figure:: /_static/images/7-Bash-secgroup-explicit.png
  :width: 800

  Figure 1. Define security group explicitly

* Drop one or more **SecurityGroupRule** component on the **SecurityGroup**. The figure below shows an inbound rule on port :code:`8080` from the remote :code:`0.0.0.0/0`.

.. figure:: /_static/images/7-Bash-secgroup-rule-explicit.png
  :width: 800

  Figure 2. An example of security group rule