.. _openshift:

******************
OpenShift template
******************

1. About
========

The following tutorial shows you how to register a (trial) subscription key from Red Hat and uses it to create an OpenShift cluster using the OpenShift template.

.. important::
  * The OpenShift template deploys a `Self-managed OpenShift Container Platform <https://www.redhat.com/en/technologies/cloud-computing/openshift/container-platform>`_ on Open Telekom Cloud using your subscription license key. Once installed, Red Hat OpenShift uses Kubernetes Operators for push-button, automatic platform updates for the container host, Kubernetes cluster, and application services running on the cluster. It also includes the following built-in technologies: Knative, Istio, Prometheus, Jenkins, Node.js, Operators, and Red Hat AMQ Streams.
  * Your subscription will cover technical support from Red Hat and upgrades between OpenShift versions. `Read more <https://www.redhat.com/en/about/value-of-Red-Hat>`_.
  * Supported versions: 4.12.39 and 4.13.x.

2. How to use
=============

2.1. How to deploy
------------------

1. Create a new application using the template **OpenShift** or **OpenShift HA** with a selected version (e.g., 4.13.19)
2. Go to **Deploy Setup**.

2.2. Deloy Setup
----------------

a. Specify base_domain
^^^^^^^^^^^^^^^^^^^^^^

Specify the **base_domain** (e.g., :code:`tri-test.com`). This is the domain name that you will use to access the OpenShift console after the deployment completes. A DNS Public Zone will be created on Open Telekom Cloud with this name. Therefore this domain name **must be unique** in the Domain Name Service of Open Telekom Cloud.

.. figure:: /_static/images/service-catalogs/openshift_config1.png
  :width: 700

  Figure 1. Specify a domain name

b. Specify pull_secret
^^^^^^^^^^^^^^^^^^^^^^

1. Register `a trial account at Red Hat <https://www.redhat.com/en/technologies/cloud-computing/openshift/ocp-self-managed-trial>`_.
2. Go to the `Redhat Console <https://console.redhat.com/openshift>`_ and copy the pull secret in Section **Downloads** / **Tokens**.

.. figure:: /_static/images/service-catalogs/openshift_redhat_console.png
  :width: 700

  Figure 2. Copy pull secret

3. Paste the content in the **pull_secret** in the Section **Secrets Inputs**.

.. figure:: /_static/images/service-catalogs/openshift_pull_secrect.png
  :width: 700

  Figure 2. Paste the pull secret

c. Specify os_password
^^^^^^^^^^^^^^^^^^^^^^

* Specfiy the **os_password**. This is the password when you login to Cloud Create.

.. note:: We do not store your password but the OpenShift install needs it one time for the installation process. In the next release, we will replace this password with an application credential for password protection.

d. (Optional) Specify ssh_public_key
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Specify the **ssh_public_key** with your SSH public key (e.g., :code:`ssh-ed25519 AAAAC3N...`). This public key will be injected in the bastion host, master and worker nodes so that you can SSH to them later on.
* If ssh_public_key is **not specified**, we will auto-select one of your **existing key pair** from the Open Telekom Cloud console instead.

.. figure:: /_static/images/service-catalogs/openshift_config2.png
  :width: 700

  Figure 3. Specify your SSH public key

e. (Optional) Specify other paramters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Specify the **number_workers** (e.g., 2). OpenShift requires a minimum of 2 worker nodes in total.
2. Specify the **nat_gateway_specs** (e.g., Small). This is the flavor of the NAT Gateway for outgoing traffic.
3. Specify the **worker_num_cpus** (e.g., 4) and **worker_mem_size** (e.g., 16 GB). OpenShift requires a minimum of 4vCPU and 16 GB memory for the worker node.

.. figure:: /_static/images/service-catalogs/openshift_config3.png
  :width: 700

  Figure 4. Specify your SSH public key

3. Expect result
================

* It takes about 2 minutes to create all compute resources on Open Telekom Cloud. Afterwards, the OpenShift bootstrap process continues to setup the master and worker nodes.
* After about 31 minutes, the **CheckOpenShiftStatus** job checks the OpenShift boostrap process and reports the status.

.. figure:: /_static/images/service-catalogs/openshift_check_result.png
  :width: 700

  Figure 5. CheckOpenShiftStatus waits 31 minutes and checks the status

3.1. Access the console
-----------------------

After the deployment completes, you can access the OpenShift console as follows.

* Copy **console_hostname**, **oauth_hostanme**, and the **INGRESS_VIP** from the deployment outputs.

.. figure:: /_static/images/service-catalogs/openshift_result1.png
  :width: 700

  Figure 6. Copy the hostname and VIP address

* Paste **console_hostname** and **oauth_hostanme** and the **INGRESS_VIP** in your **/etc/hosts**

.. code-block:: bash

  # vim /etc/hosts
  80.158.36.243 console-openshift-console.apps.openshift.tri-test.com
  80.158.36.243 oauth-openshift.apps.openshift.tri-test.com

* Access the OpenShift console URL via the web browser with the **kubeadmin_username** and **kubeadmin_password** from the deployment outputs.

.. code-block:: bash

  # The output of the "console_url"
  https://console-openshift-console.apps.openshift.tri-test.com

.. figure:: /_static/images/service-catalogs/openshift_result2.png
  :width: 700

  Figure 7. Access the OpenShift console

3.2. Access the bastion host
----------------------------

During the OpenShift bootstrap process, you can access to the bastion host as follows:

* Copy **public_address** of the **Bastionhost**

.. figure:: /_static/images/service-catalogs/openshift_result3.png

  Figure 8. The public IP address of the bastion host

* Access the bastion host with the IP

.. code-block:: bash

  # We use Ubuntu OS for the bastion host
  ssh ubuntu@164.30.10.109

* Check nodes are ready

.. code-block:: bash

  # Set KUBECONFIG
  export KUBECONFIG="/home/ubuntu/openshift/auth/kubeconfig"

  # Check all nodes are ready
  $ oc get nodes
  NAME                        STATUS   ROLES                  AGE    VERSION
  openshift-k55b9-master-0    Ready    control-plane,master   179m   v1.26.9+636f2be
  openshift-k55b9-master-1    Ready    control-plane,master   179m   v1.26.9+636f2be
  openshift-k55b9-master-2    Ready    control-plane,master   179m   v1.26.9+636f2be
  openshift-k55b9-workers-0   Ready    worker                 163m   v1.26.9+636f2be
  openshift-k55b9-workers-1   Ready    worker                 163m   v1.26.9+636f2be

* Check all cluster operators are available

.. code-block:: bash

  $ oc get clusteroperators
    NAME                                       VERSION   AVAILABLE   PROGRESSING   DEGRADED   SINCE   MESSAGE
    authentication                             4.13.19   True        False         False      156m
    baremetal                                  4.13.19   True        False         False      174m
    cloud-controller-manager                   4.13.19   True        False         False      3h1m
    cloud-credential                           4.13.19   True        False         False      3h4m
    cluster-autoscaler                         4.13.19   True        False         False      175m
    config-operator                            4.13.19   True        False         False      175m
    console                                    4.13.19   True        False         False      161m
    control-plane-machine-set                  4.13.19   True        False         False      175m
    csi-snapshot-controller                    4.13.19   True        False         False      175m
    dns                                        4.13.19   True        False         False      174m
    etcd                                       4.13.19   True        False         False      174m
    image-registry                             4.13.19   True        False         False      163m
    ingress                                    4.13.19   True        False         False      163m
    insights                                   4.13.19   True        False         False      168m
    kube-apiserver                             4.13.19   True        False         False      164m
    kube-controller-manager                    4.13.19   True        False         False      172m
    kube-scheduler                             4.13.19   True        False         False      172m
    kube-storage-version-migrator              4.13.19   True        False         False      164m
    machine-api                                4.13.19   True        False         False      171m
    machine-approver                           4.13.19   True        False         False      174m
    machine-config                             4.13.19   True        False         False      174m
    marketplace                                4.13.19   True        False         False      174m
    monitoring                                 4.13.19   True        False         False      162m
    network                                    4.13.19   True        False         False      177m
    node-tuning                                4.13.19   True        False         False      174m
    openshift-apiserver                        4.13.19   True        False         False      165m
    openshift-controller-manager               4.13.19   True        False         False      174m
    openshift-samples                          4.13.19   True        False         False      168m
    operator-lifecycle-manager                 4.13.19   True        False         False      174m
    operator-lifecycle-manager-catalog         4.13.19   True        False         False      175m
    operator-lifecycle-manager-packageserver   4.13.19   True        False         False      169m
    service-ca                                 4.13.19   True        False         False      175m
    storage                                    4.13.19   True        False         False      170m