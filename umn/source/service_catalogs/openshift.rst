.. _openshift:

******************
OpenShift template
******************

1. About
========

The following tutorial shows you how to register a (trial) subscription key from Red Hat and uses it to create an OpenShift cluster using the OpenShift template.

.. important::
  * The OpenShift template deploys a `Self-managed OpenShift Container Platform <https://www.redhat.com/en/technologies/cloud-computing/openshift/container-platform>`_ on Open Telekom Cloud (OTC) with Bring Your Own License (BYOL).
  * Your license/subscription will cover technical support from Red Hat as well as upgrades between OpenShift versions. `Read more <https://www.redhat.com/en/about/value-of-Red-Hat>`_.
  * Versions available in the template: :code:`4.12.39`, :code:`4.13.19`, :code:`4.16.19`. Please contact us if you need a specific version.

2. How to use
=============

2.1. How to deploy
------------------

1. Create a new application using the template **OpenShift** or **OpenShift HA** with a selected version (e.g., 4.16.19)
2. Go to **Quick Deploy**.

2.2. Deloy Setup
----------------

a. Specify base_domain
^^^^^^^^^^^^^^^^^^^^^^

Specify the **base_domain** (e.g., :code:`tri-test.com`). This is the domain name that you will use to access the OpenShift console after the deployment completes. A DNS Public Zone will be created on OTC with this name. Therefore this domain name **must be unique** in the Domain Name Service of OTC.

.. figure:: /_static/images/service-catalogs/openshift_config1.png
  :width: 700

  Figure 1. Specify a domain name

.. important::

  **Swiss OTC** does not support a DNS Public Zone. However, you can bring your own domain name and set it here. For example, you can register a free domain name on ClouDNS (e.g., tri-test.ddns-ip.net). Then you set it to the base_domain. After the deployment completes, you can use this domain name to access the OpenShift console. See instruction in :ref:`access with nameservers`.


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
* If ssh_public_key is **not specified**, we will auto-select one of your **existing key pair** from the OTC console instead.

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

* It takes about 2 minutes to create all compute resources on OTC. Afterwards, the OpenShift bootstrap process continues to setup the master and worker nodes.
* After about 31 minutes, the **CheckOpenShiftStatus** job checks the OpenShift boostrap process and reports the status.

.. figure:: /_static/images/service-catalogs/openshift_check_result.png
  :width: 700

  Figure 5. CheckOpenShiftStatus waits 31 minutes and checks the status

3.1. Access the console
-----------------------

First, you need to resolve the hostname of the OpenShift console as follows.

Option 1. Add hostname in /etc/host
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Copy **console_hostname**, **oauth_hostanme**, and the **INGRESS_VIP** from the deployment outputs.

.. figure:: /_static/images/service-catalogs/openshift_result1.png
  :width: 700

  Figure 6. Copy the hostname and VIP address

* Paste **console_hostname** and **oauth_hostanme** and the **INGRESS_VIP** in your **/etc/hosts**

.. code-block:: bash

  # vim /etc/hosts
  80.158.36.243 console-openshift-console.apps.openshift.tri-test.com
  80.158.36.243 oauth-openshift.apps.openshift.tri-test.com

.. _access with nameservers:

Option 2. Add nameservers
^^^^^^^^^^^^^^^^^^^^^^^^^

On OTC, a DNS public zone is created with the record sets type A pointing to the ingress VIP address of the OpenShift cluster as follows:

.. figure:: /_static/images/service-catalogs/openshift-dns.png
  :width: 900

  Figure 7. A DNS public zone is created on OTC

It means, if you add the nameservers :code:`ns1.open-telekom-cloud.com` (80.158.48.19) or :code:`ns2.open-telekom-cloud.com` (93.188.242.252) to your machine, you can access the OpenShift console URL.

.. important::
  **Swiss OTC** does not support a DNS Public Zone. However, you can register a DNS somewhere else. For example, first you register a domain name on a free DNS like `ClouDNS <https://www.cloudns.net>`_ (e.g., :code:`tri-test.ddns-ip.net`). Then you set it to the input **base_domain** (in Step 2.2a). Finally, you set a record set type A on ClouDNS pointing to the ingress VIP of the OpenShift cluster:

  .. figure:: /_static/images/service-catalogs/openshift-dns2.png
    :width: 600

    Figure 8. An example with a free DNS on ClouDNS

Now you can access the OpenShift console URL via the web browser with the **kubeadmin_username** and **kubeadmin_password** from the deployment outputs.

.. code-block:: bash

  # The output of the "console_url"
  https://console-openshift-console.apps.openshift.tri-test.com

.. figure:: /_static/images/service-catalogs/openshift_result2.png
  :width: 700

  Figure 9. Access the OpenShift console

3.2. Access the bastion host
----------------------------

During the OpenShift bootstrap process, you can access to the bastion host as follows:

* Copy **public_address** of the **Bastionhost**

.. figure:: /_static/images/service-catalogs/openshift_result3.png

  Figure 10. The public IP address of the bastion host

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

4. Post-installation
====================

4.1. TODO after installation
----------------------------

4.1.1. Change kubeadmin password
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Cloud Create auto-generates the kubeadmin password in plaintext for you. Log in the OpenShift console and change it.

4.1.2. Delete bootstrap resources
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The **bootstrap** VM is only needed during the installation. After the installation completes, you can delete it via the Web console.

.. figure:: /_static/images/service-catalogs/openshift-delete-bootstrap.png
  :width: 900

  Figure 11. Go to Web console and delete the VM "bootstrap"

4.2. Maintaining credentials
----------------------------

On OTC, a user password is expired every 3 months (by default). After it is expired, OpenShift cannot authenticate to OTC to provision volumes so you may get the following error:

.. code-block:: bash

  MountVolume.SetUp failed for volume "pvc-xxx" : rpc error: code = Internal desc = GetVolume failed with error Unable to re-authenticate:
  Expected HTTP response code [200] when accessing [GET https://evs.eu-de.otc.t-systems.com/v3/yyy/volumes/zzz],
  but got 401 instead Authentication required: Authentication failed

**Solution**

1. Go to the Web Console and update your password on in the Section **Security Settings**.

.. tip:: You can increase the password expired time in the Section **Password Policy**.

2. Update OpenShift with your new password:

* Go to the OpenShift Console.
* Go to **Workloads** / **Secrets**.
* Find and edit the secret **openstack-credentials**.
* Update the value **password** in both “clouds.conf” and “clouds.yaml”.

.. figure:: /_static/images/service-catalogs/openshift-update-password.png
  :width: 900

  Figure 12. Update the secret openstack-credentials


5. How to create storages
=========================

5.1. Elastic Volume Service (EVS)
---------------------------------

In OpenShift you can provision an EVS on OTC dynamically:

1. Create a new **storage class** (e.g., :code:`ssd-csi`) with a volume type (e.g., :code:`SSD`):

.. code-block:: yaml

    apiVersion: storage.k8s.io/v1
    kind: StorageClass
    metadata:
      name: ssd-csi
    provisioner: cinder.csi.openstack.org
    parameters:
      type: SSD # Choose 'SSD' for 'Ultra high I/O', 'SAS' for 'High I/O', 'SATA' for 'Common I/O'
    reclaimPolicy: Delete
    allowVolumeExpansion: true
    volumeBindingMode: WaitForFirstConsumer # PVC is PENDING until the Pod is created. As a result, the volume is created in the same AZ as the POD.

(Alternative) Create a storage class with specific AZ (e.g., :code:`eu-de-01`) so that volumes will be created only in this AZ:

.. code-block:: yaml

    apiVersion: storage.k8s.io/v1
    kind: StorageClass
    metadata:
      name: eu-de-01-ssd-csi
    provisioner: cinder.csi.openstack.org
    parameters:
      type: SSD # Choose 'SSD' for 'Ultra high I/O', 'SAS' for 'High I/O', 'SATA' for 'Common I/O'
    reclaimPolicy: Delete
    allowVolumeExpansion: true
    allowedTopologies:
    - matchLabelExpressions:
      - key: topology.cinder.csi.openstack.org/zone
        values:
        - eu-de-01 # Choose 'eu-de-01', 'eu-de-02', 'eu-de-03'

2. Create a **PersistentVolumeClaim** (e.g., :code:`ssd-pvc`) with the storage class :code:`ssd-csi`:

.. code-block:: yaml

    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: ssd-pvc
      namespace: <YOUR_NAMESPACE>
    spec:
      storageClassName: ssd-csi
      accessModes:
        - ReadWriteOnce
      volumeMode: Filesystem
      resources:
        requests:
          storage: 10Gi

3. Create a Pod :code:`example` with the PersistentVolumeClaim :code:`ssd-pvc`:

.. code-block:: yaml

    apiVersion: v1
    kind: Pod
    metadata:
      name: example
      labels:
        app: httpd
      namespace: <YOUR_NAMESPACE>
    spec:
      securityContext:
        runAsNonRoot: true
        seccompProfile:
          type: RuntimeDefault
      containers:
        - name: httpd
          image: 'image-registry.openshift-image-registry.svc:5000/openshift/httpd:latest'
          ports:
            - containerPort: 8080
          securityContext:
            allowPrivilegeEscalation: false
            capabilities:
              drop:
                - ALL
          volumeMounts: # Add the following lines to the 'example' Pod to test the PVC
            - name: ssd-volume
              mountPath: /test
      volumes:
        - name: ssd-volume
          persistentVolumeClaim:
            claimName: ssd-pvc



4. On OpenShift console, see Pod is running:

.. figure:: /_static/images/service-catalogs/openshift_pod.png

  Figure 13. Pod example is running

5. On OTC, see EVS is created:

.. figure:: /_static/images/service-catalogs/openshift_evs.png

  Figure 14. A new EVS is created with the volume type "Ultra High I/O"

5.2. Scalable File Service & SFS Turbo
--------------------------------------

You can create a SFS on OTC manually and create a `PersistentVolume using NFS <https://docs.openshift.com/container-platform/4.13/storage/persistent_storage/persistent-storage-nfs.html>`_ in OpenShift, which connects to SFS via NFS protocol:

1. Go to the `webconsole of OTC <https://console.otc.t-systems.com/>`_ and create a SFS or SFS Turbo:

.. figure:: /_static/images/service-catalogs/openshift_sfs.png
  :width: 900

  Figure 15. Create SFS via webconsole

* Choose the VPC and subnet of your OpenShift so that the SFS is created in the same subnet. The VPC :code:`cc-environment-openshift00` in this example was created by Cloud Create, which starts with the prefix :code:`cc`, followed by the environement name :code:`enviroment` and the application name :code:`openshift00`.
* Choose the security group `sg-worker`. This is the security group of the worker nodes.

2. Copy the SFS endpoint

.. figure:: /_static/images/service-catalogs/openshift_sfs2.png

  Figure 16. Copy the SFS endpoint :code:`10.0.207.136`

3. Create a PersistentVolume (e.g., :code:`sfs-pv`) with the SFS endpoint:

.. code-block:: yaml

    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: sfs-pv
    spec:
      capacity:
        storage: 500Gi
      accessModes:
      - ReadWriteMany
      nfs:
        server: 10.0.207.136 # SFS endpoint
        path: /
      persistentVolumeReclaimPolicy: Retain

4. Create a PersistentVolumeClaim (e.g., :code:`sfs-pvc`) with the :code:`sfs-pv`:

.. code-block:: yaml

    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: sfs-pvc
      namespace: <YOUR_NAMESPACE>
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 500Gi
      volumeName: sfs-pv
      storageClassName: "" # Important

5. Create a Pod to use :code:`sfs-pvc`

6. Tear down
============

* In Cloud Create, go to **Action** / **Undeploy** to delete the OpenShift cluster.
* The PVC storages, which were created by OpenShift, will not be deleted automatically. You have to delete them manually.

.. figure:: /_static/images/service-catalogs/openshift_tear_down.png

  Figure 17. Check PVC with Available status

7. Links
========

* Our `OpenShift app template in TOSCA <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs/blob/main/templates/openshift/4.13/topology.yml>`_.
* How to create a `PersistentVolume using NFS in OpenShift <https://docs.openshift.com/container-platform/4.13/storage/persistent_storage/persistent-storage-nfs.html>`_.
* `Maintaining credentials in OpenShift <https://docs.openshift.com/container-platform/4.14/post_installation_configuration/changing-cloud-credentials-configuration.html#manually-rotating-cloud-creds_changing-cloud-credentials-configuration>`_.