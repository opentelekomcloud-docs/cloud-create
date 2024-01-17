**************
Ansible Galaxy
**************

The following example shows how to deploy a web application on Tomcat using the **AnsibleGalaxy** component. We will use the galaxy `zaxos.tomcat-ansible-role <https://galaxy.ansible.com/zaxos/tomcat-ansible-role>`_ to deploy Tomcat.

.. figure:: /_static/images/8-Ansible-galaxy.png
  :width: 800

  Figure 1. Ansible Galaxy example

.. tip:: You can find this example under New Application / Topology Template / Ansible-Galaxy.

Step 1. Create compute
======================

* Design a compute with access to public network.
* Set **distribution** to :code:`centOS`.

.. figure:: /_static/images/8-Ansible-galaxy-compute.png
  :width: 800

  Figure 2. Create a compute with public access

The ansible galaxy in this example only works on CentOS/RHEL 7 so we have to set the distribution for the compute node.

Step 2. Create AnsibleGalaxy Tomcat
===================================

* Drop the **AnsibleGalaxy** on the compute. Name it :code:`Tomcat`.
* Set **galaxy_name** to :code:`zaxos.tomcat-ansible-role`. This is the name of the galaxy to install.
* Set **component_version** to :code:`1.3.2`. This is the tag of the GIT repository `zaxos.tomcat-ansible-role <https://galaxy.ansible.com/zaxos/tomcat-ansible-role>`_. By default, it is set to :code:`master`.
* Set **port** to :code:`8080`. The orchestrator will create a security group to allow incoming traffic on port 8080 automatically.

.. figure:: /_static/images/8-Ansible-galaxy-property.png
  :width: 800

  Figure 3. Ansible Galaxy properties

Step 3. Define ansible variables
================================

* Write a yml file (e.g., :code:`tomcat_vars.yml`):

.. code-block:: yaml

  # In this example, we configure the ansible galaxy to deploy Tomcat at :code:`/opt`
  # and listen on port :code:`8080`.
  tomcat_version: 8.5.23
  tomcat_install_path: /opt
  tomcat_port_connector: 8080
  tomcat_permissions_production: True

  tomcat_users:
    - username: "tomcat"
      password: "t3mpp@ssw0rd"
      roles: "tomcat,admin,manager,manager-gui"

.. seealso::

  The ansible variables above are from the galaxy `zaxos.tomcat-ansible-role <https://galaxy.ansible.com/zaxos/tomcat-ansible-role>`_. Take a look at the given galaxy to see which variables are available and customize them according to your needs.

* Upload :code:`tomcat_vars.yml` and select it as the artifact **ansible_variables**.

.. figure:: /_static/images/8-Ansible-galaxy-variables.png
  :width: 800

  Figure 4. Ansible Galaxy variables

Step 4. Configure AnsibleGalaxy
===============================

* By default, Tomcat uses Ipv6, so we write an ansible task (e.g., :code:`setenv.yml`) to configure Tomcat to listen on IPv4:

.. code-block:: yaml

  - name: "Tell tomcat to listen for ipv4 instead of ipv6"
    copy:
      dest: "/opt/tomcat/bin/setenv.sh"
      content: |
        JAVA_OPTS="$JAVA_OPTS -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Addresses=true"
      owner: tomcat
      group: tomcat
  - name: Restart tomcat to enable ipv4
    systemd:
      state: restarted
      daemon_reload: yes
      name: tomcat
  - name: "Flush firewall (optional)"
    shell: iptables -F INPUT
    args:
      executable: /bin/bash

* Upload :code:`setenv.yml` and select it as the artifact **configure**.

.. note:: This example shows how to execute additional tasks after the ansible galaxy is applied.

Step 5. Deploy the .war file
============================

* Drop **GetUrl** on the **AnsibleGalaxy** component. This component downloads a file on the Compute node after the Ansible Galaxy has completed.
* Check the box **ansible_become**. This makes sure the file is created on the Compute node with no permission issues.
* Set **url** to :code:`https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/sample.war`. This will download the :code:`sample.war` file from the given url. Here we use the :code:`sample.war` file from `the Tomcat sample web application <https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/>`_.
* Set **dest** to :code:`/opt/tomcat/webapps`. This is the :code:`webapps` location inside the :code:`CATALINA_HOME` of Tomcat. The :code:`sample.war` file will be created in this directory.
* Set **group** to :code:`tomcat`. This will set the permission of the :code:`sample.war` file to the group tomcat. The ansible galaxy `zaxos.tomcat-ansible-role <https://galaxy.ansible.com/zaxos/tomcat-ansible-role>`_ will create this group for you beforehand.

.. figure:: /_static/images/8-Ansible-geturl.png
  :width: 800

  Figure 5. Using geturl

Step 6. Define the deployment output (optional)
===============================================

* Select **app_url** as outputs properties. The deployment will output the web application URL for you.

.. figure:: /_static/images/8-Ansible-galaxy-output.png
  :width: 800

  Figure 5. Ansible Galaxy output

Expected result
===============

* Access the app_url :code:`http://<public_ip>:8080/sample/`

.. figure:: /_static/images/8-Ansible-galaxy-sample-app.png
  :width: 800

  Figure 6. Sample web application