.. _Hardening:

*********
Hardening
*********

1. About
========

Hardening is the process of securing a system by reducing vulnerability and available ways of attack. For example: If your server is not a desktop system, then Xorg, KDE/GNOME/Unity must be disabled.

If you on-boarding your applications on Open Telekom Cloud, you may know about the `Privacy Security Assessment (PSA) process <https://www.telekom.com/en/corporate-responsibility/data-protection-data-security/security/details/privacy-and-security-assessment-process-358312>`_. These are hundreds of security guidelines from Deutsche Telekom that your applications should be fulfilled.

This service catalog (when putting on a compute node) automates the security requirements of Deutsche Telekom on the booted VM.

2. How to use
=============

* Put the component :code:`SshHardening` (and/or :code:`OsHardening`) on a compute node.
* Customize the property as needed (e.g., enable/disable SSH agent forwarding).

.. figure:: /_static/images/service-catalogs/ssh-hardning.png
  :width: 800

  Figure 1. SSH Hardening

The default properties enforce hardening on the VM following the `SSH Baseline <https://dev-sec.io/baselines/ssh/>`_ and the `Linux Security Baseline <https://dev-sec.io/baselines/linux/>`_ from the DevSec project so you do not need to do anything further. For example, when putting the :code:`SshHardening` on a compute node, SSH agent forwarding is disabled on the VM by default as it can be used in a limited way to enable attacks (See the requirement **ssh-11** from the `SSH Baseline <https://dev-sec.io/baselines/ssh/>`_).

3. About the DevSec project
===========================

Deutsche Telekom, T-Labs, and Telekom Security funded the initial research of this project and open source the automation to help foster a more secure world. This service catalog uses the ansible implementation of this project.

4. Links
========

* See `how this service catalog is modelled in TOSCA format <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs/blob/main/hardening/types.yaml>`_.
* See the implementation of `OS hardening <https://github.com/dev-sec/ansible-collection-hardening/tree/master/roles/os_hardening>`_ and `SSH hardening <https://github.com/dev-sec/ansible-collection-hardening/tree/master/roles/ssh_hardening>`_ from DevSec to understand how the hardening is applied at runtime.