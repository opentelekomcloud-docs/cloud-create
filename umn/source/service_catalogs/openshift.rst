.. _openshift:

******************
OpenShift template
******************

1. About
========

The following tutorial shows you how to register a (trial) subscription key from Red Hat and uses it to create an OpenShift cluster using the OpenShift template.

.. important::
  * This template deploys a `Self-managed OpenShift Container Platform <https://www.redhat.com/en/technologies/cloud-computing/openshift/container-platform>`_ on Open Telekom Cloud (OTC) with Bring Your Own License (BYOL). It follows the same setup from `the documentation OpenStack User Provisioned Infrastructure <https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/installing_on_openstack/installing-openstack-user>`_.
  * Your license/subscription will cover technical support from Red Hat as well as upgrades between OpenShift versions. `Read more <https://www.redhat.com/en/about/value-of-Red-Hat>`_.
  * Versions available in the template: :code:`4.16.19` and :code:`4.18.30`. Please contact us if you need a specific version.

2. How to use
=============

2.1. How to deploy
------------------

1. Create a new application using the template **OpenShift** or **OpenShift HA** with a selected version (e.g., 4.18.30)
2. Go to **Quick Deploy**.

.. figure:: /_static/images/service-catalogs/openshift_select_template.png
  :width: 700

  Figure 1. Choose the template OpenShift

2.2. Deloy Setup
----------------

a. Specify base_domain
^^^^^^^^^^^^^^^^^^^^^^

Specify the **base_domain** (e.g., :code:`tri-test.com`). This is the domain name that you will use to access the OpenShift console after the deployment completes. You can specify any domain names you prefer. Your domain name will be registered on the nameservers of OTC. After the deployment completes, you can add the nameservers of OTC on your local machine, then the domain can be resolved (See Section :ref:`access_openshift_console`). Alternatively, you can register a public domain name (e.g., a free domain from ClouDNS) and input it here. In the later case, you do not need to add the nameservers of OTC.

.. figure:: /_static/images/service-catalogs/openshift_config1.png
  :width: 700

  Figure 2. Specify a domain name

.. important::

  **Swiss OTC** does not support a DNS Public Zone. However, you can bring your own domain name and set it here. For example, you can register a free domain name on ClouDNS (e.g., tri-test.ddns-ip.net). Then you set it to the base_domain. After the deployment completes, you can use this domain name to access the OpenShift console. See instruction in :ref:`access with nameservers`.


b. Specify pull_secret
^^^^^^^^^^^^^^^^^^^^^^

1. Register `a trial account at Red Hat <https://www.redhat.com/en/technologies/cloud-computing/openshift/ocp-self-managed-trial>`_.
2. Go to the `Redhat Console <https://console.redhat.com/openshift>`_ and copy the pull secret in Section **Downloads** / **Tokens**.

.. figure:: /_static/images/service-catalogs/openshift_redhat_console.png
  :width: 700

  Figure 3. Copy pull secret

3. Paste the content in the **pull_secret** in the Section **Secrets Inputs**.

.. figure:: /_static/images/service-catalogs/openshift_pull_secrect.png
  :width: 700

  Figure 4. Paste the pull secret

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

  Figure 5. Specify your SSH public key

e. (Optional) Specify other paramters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Specify the **number_workers** (e.g., 2). OpenShift requires a minimum of 2 worker nodes in total.
2. Specify the **nat_gateway_specs** (e.g., Small). This is the flavor of the NAT Gateway for outgoing traffic.
3. Specify the **worker_num_cpus** (e.g., 4) and **worker_mem_size** (e.g., 16 GB). OpenShift requires a minimum of 4vCPU and 16 GB memory for the worker node.

.. figure:: /_static/images/service-catalogs/openshift_config3.png
  :width: 700

  Figure 6. Specify your SSH public key

3. Expect result
================

* It takes about 2 minutes to create all compute resources on OTC. Afterwards, the OpenShift bootstrap process continues to setup the master and worker nodes.
* After about 31 minutes, the **CheckOpenShiftStatus** job checks the OpenShift boostrap process and reports the status.

.. figure:: /_static/images/service-catalogs/openshift_check_result.png
  :width: 700

  Figure 7. CheckOpenShiftStatus waits 31 minutes and checks the status

.. _access_openshift_console:

3.1. Access the console
-----------------------

First, you need to resolve the hostname of the OpenShift console as follows.

Option 1. Add hostname in /etc/host
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Copy **console_hostname**, **oauth_hostanme**, and the **INGRESS_VIP** from the deployment outputs.

.. figure:: /_static/images/service-catalogs/openshift_result1.png
  :width: 700

  Figure 8. Copy the hostname and VIP address

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

  Figure 9. A DNS public zone is created on OTC

It means, if you add the nameservers :code:`ns1.open-telekom-cloud.com` (80.158.48.19) or :code:`ns2.open-telekom-cloud.com` (93.188.242.252) to your machine, you can access the OpenShift console URL.

.. important::
  **Swiss OTC** does not support a DNS Public Zone. However, you can register a DNS somewhere else. For example, first you register a domain name on a free DNS like `ClouDNS <https://www.cloudns.net>`_ (e.g., :code:`tri-test.ddns-ip.net`). Then you set it to the input **base_domain** (in Step 2.2a). Finally, you set a record set type A on ClouDNS pointing to the ingress VIP of the OpenShift cluster:

  .. figure:: /_static/images/service-catalogs/openshift-dns2.png
    :width: 600

    Figure 10. An example with a free DNS on ClouDNS

Now you can access the OpenShift console URL via the web browser with the **kubeadmin_username** and **kubeadmin_password** from the deployment outputs.

.. code-block:: bash

  # The output of the "console_url"
  https://console-openshift-console.apps.openshift.tri-test.com

.. figure:: /_static/images/service-catalogs/openshift_result2.png
  :width: 700

  Figure 11. Access the OpenShift console

3.2. Access the bastion host
----------------------------

During the OpenShift bootstrap process, you can access to the bastion host as follows:

* Copy **public_address** of the **Bastionhost**

.. figure:: /_static/images/service-catalogs/openshift_result3.png

  Figure 12. The public IP address of the bastion host

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
  NAME                             STATUS   ROLES                  AGE    VERSION
  openshift-k55b9-master-0         Ready    control-plane,master   111m   v1.31.14
  openshift-k55b9-master-1         Ready    control-plane,master   109m   v1.31.14
  openshift-k55b9-master-2         Ready    control-plane,master   111m   v1.31.14
  openshift-k55b9-workers-0        Ready    worker                 92m    v1.31.14
  openshift-k55b9-workers-1        Ready    worker                 89m    v1.31.14

* Check all cluster operators are available

.. code-block:: bash

  $ oc get clusteroperators
  NAME                                       VERSION   AVAILABLE   PROGRESSING   DEGRADED   SINCE   MESSAGE
  authentication                             4.18.30   True        False         False      78m
  baremetal                                  4.18.30   True        False         False      102m
  cloud-controller-manager                   4.18.30   True        False         False      109m
  cloud-credential                           4.18.30   True        False         False      111m
  cluster-autoscaler                         4.18.30   True        False         False      102m
  config-operator                            4.18.30   True        False         False      103m
  console                                    4.18.30   True        False         False      84m
  control-plane-machine-set                  4.18.30   True        False         False      102m
  csi-snapshot-controller                    4.18.30   True        False         False      103m
  dns                                        4.18.30   True        False         False      102m
  etcd                                       4.18.30   True        False         False      101m
  image-registry                             4.18.30   True        False         False      88m
  ingress                                    4.18.30   True        False         False      88m
  insights                                   4.18.30   True        False         False      102m
  kube-apiserver                             4.18.30   True        False         False      99m
  kube-controller-manager                    4.18.30   True        False         False      99m
  kube-scheduler                             4.18.30   True        False         False      99m
  kube-storage-version-migrator              4.18.30   True        False         False      91m
  machine-api                                4.18.30   True        False         False      99m
  machine-approver                           4.18.30   True        False         False      102m
  machine-config                             4.18.30   True        False         False      102m
  marketplace                                4.18.30   True        False         False      102m
  monitoring                                 4.18.30   True        False         False      82m
  network                                    4.18.30   True        False         False      106m
  node-tuning                                4.18.30   True        False         False      87m
  olm                                        4.18.30   True        False         False      93m
  openshift-apiserver                        4.18.30   True        False         False      93m
  openshift-controller-manager               4.18.30   True        False         False      91m
  openshift-samples                          4.18.30   True        False         False      93m
  operator-lifecycle-manager                 4.18.30   True        False         False      102m
  operator-lifecycle-manager-catalog         4.18.30   True        False         False      102m
  operator-lifecycle-manager-packageserver   4.18.30   True        False         False      94m
  service-ca                                 4.18.30   True        False         False      103m
  storage                                    4.18.30   True        False         False      91m

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

  Figure 13. Go to Web console and delete the VM "bootstrap"

4.2. Update os_password
-----------------------

On OTC, an IAM user password is expired every 3 months (by default). After it is expired, OpenShift cannot authenticate to OTC to provision volumes so you may get the following error:

.. code-block:: bash

  MountVolume.SetUp failed for volume "pvc-xxx" : rpc error: code = Internal desc = GetVolume failed with error Unable to re-authenticate:
  Expected HTTP response code [200] when accessing [GET https://evs.eu-de.otc.t-systems.com/v3/yyy/volumes/zzz],
  but got 401 instead Authentication required: Authentication failed

**Solution**

1. Go to the Web Console and update the IAM user password under **Security Settings** / **Basic Information** / **Login password**:

.. figure:: /_static/images/service-catalogs/openshift-update-os-password.png
  :width: 900

  Figure 14. Update password on the Web console

.. tip:: You can increase the password expired time in the Section **Password Policy**.

2. Update OpenShift with your new password:

* Go to the OpenShift Console.
* Go to **Workloads** / **Secrets**.
* Find and edit the secret **openstack-credentials** in the **kube-system** namespace.
* Update the value **password** in 2 files **clouds.conf** and **clouds.yaml**.

.. figure:: /_static/images/service-catalogs/openshift-update-password.png
  :width: 900

  Figure 15. Update the secret openstack-credentials

The Cloud Credential Operator (CCO) will `rotate the secret <https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/postinstallation_configuration/changing-cloud-credentials-configuration#manually-rotating-cloud-creds_changing-cloud-credentials-configuration>`_. The other secrets **openstack-cloud-credentials** (in Figure 15) will be eventually overwritten. The rotation may take a few minutes.

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
      type: SSD # Choose 'SSD' for Ultra-high I/O type, 'GPSSD' for the general purpose SSD type, 'ESSD' for the extreme SSD type, 'SAS' for High I/O type
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
      type: SSD # Choose 'SSD' for Ultra-high I/O type, 'GPSSD' for the general purpose SSD type, 'ESSD' for the extreme SSD type, 'SAS' for High I/O type
    reclaimPolicy: Delete
    allowVolumeExpansion: true
    allowedTopologies:
    - matchLabelExpressions:
      - key: topology.cinder.csi.openstack.org/zone
        values:
        - eu-de-01 # Choose 'eu-de-01', 'eu-de-02', and 'eu-de-03' (for OTC), 'eu-ch2a' and 'eu-ch2b' (for Swiss OTC).

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

  Figure 16. Pod example is running

5. On OTC, see EVS is created:

.. figure:: /_static/images/service-catalogs/openshift_evs.png

  Figure 17. A new EVS is created with the volume type "Ultra High I/O"

5.2. Scalable File Service & SFS Turbo
--------------------------------------

You can create a SFS on OTC manually and create a `PersistentVolume using NFS <https://docs.openshift.com/container-platform/4.13/storage/persistent_storage/persistent-storage-nfs.html>`_ in OpenShift, which connects to SFS via NFS protocol:

1. Go to the `webconsole of OTC <https://console.otc.t-systems.com/>`_.
2. Go to **Scalable File Service** / **Create File System**

* Choose the **VPC** and **subnet** so that the SFS is created in the same subnet of the OpenShift worker nodes. During the deployment, Cloud Create created a VPC with the prefix :code:`cc`, followed by the environement name :code:`enviroment` and the application name :code:`openshift00`. In our example, the VPC is :code:`cc-environment-openshift00`. The subnet of our OpenShift worker nodes is :code:`cc-environment-openshift00-private-subnet`.
* Left the **Security group** option **unchecked** so that a security group is auto-created for the SFS.

.. figure:: /_static/images/service-catalogs/openshift_sfs.png
  :width: 900

  Figure 18. Create SFS via webconsole

2. After SFS is created successfully, it is assigned with a new security group having all necessary ports opened to :code:`0.0.0.0/0` (by default). Then you can limit the access to SFS from the OpenShift worker nodes by setting the **source** security group to :code:`sg-worker`:

.. figure:: /_static/images/service-catalogs/openshift_sfs_security_group.png
  :width: 900

  Figure 19. Security group of SFS

3. Copy the SFS endpoint

.. figure:: /_static/images/service-catalogs/openshift_sfs2.png

  Figure 20. Copy the SFS endpoint :code:`10.0.207.136`

4. Create a PersistentVolume (e.g., :code:`sfs-pv`) with the SFS endpoint:

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

5. Create a PersistentVolumeClaim (e.g., :code:`sfs-pvc`) with the :code:`sfs-pv`:

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

6. Create a Pod to use :code:`sfs-pvc`

6. MachineSet API
=================

You can use the MachineSet API from OpenShift to spawn worker nodes on OTC as follows.

6.1 Apply patch (temporal)
--------------------------

.. important::

  The `REST API of OTC to create a network port <https://docs.otc.t-systems.com/virtual-private-cloud/api-ref/native_openstack_neutron_apis_v2.0/port/creating_a_port.html>`_ is not fully compliant with the OpenStack API. Therefore, the Machine API of OpenShift fails to create network ports on OTC. Follow the **steps below** to apply the `patch <https://github.com/opentelekomcloud-blueprints/machine-api-provider-openstack/tree/4.18-patched>`_ so that the Machine API can work with the REST API of OTC. When the compliant issues are fixed, this patch is not necessary.

.. code-block:: bash

    # 1. Pause the CVO to stop it from overwriting
    $ oc scale deploy cluster-version-operator -n openshift-cluster-version --replicas=0

    # 2. Edit the configmap machine-api-operator-images
    $ oc edit configmap/machine-api-operator-images -n openshift-machine-api
    # Replace:
    "clusterAPIControllerOpenStack": "quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:8df78826ffbf29ea291cde12a4eb81b8d63103ddb15b01c5af55d22e80448989",
    # With:
    "clusterAPIControllerOpenStack": "swr.eu-de.otc.t-systems.com/cloud-create/machine-api-provider-openstack@sha256:60a7811b80265a731d45bdf47c5492398ddc6ef4da02aad6c2a7f67ab66e4247",

    # 3. Update the machine-controller to use our patched image
    $ oc -n openshift-machine-api set image deployment/machine-api-controllers machine-controller=swr.eu-de.otc.t-systems.com/cloud-create/machine-api-provider-openstack@sha256:60a7811b80265a731d45bdf47c5492398ddc6ef4da02aad6c2a7f67ab66e4247

    # 4. Wait a few minutes for the machine-controller to start
    $ oc logs -n openshift-machine-api deployment/machine-api-controllers -c machine-controller
    I1125 16:31:36.679739       1 controller.go:215] "msg"="Starting workers" "controller"="machine-controller" "worker count"=1

6.2 How to create a machineset
------------------------------

1. In the **Topology** view, click on the **OpenShiftInstall**, copy the values of :code:`cluster_os_image` (e.g., :code:`rhcos-418.94.202501221327-0-openstack.x86_64`) and :code:`infra_id` (e.g., :code:`openshift-vts4p`).

.. figure:: /_static/images/service-catalogs/openshift_machinset1.png
  :width: 600

  Figure 21. Copy cluster_os_image and infra_id

2. Click on the **Private** network and copy the **subnet_id** (e.g., :code:`1423eb8d-c552-410b-9b10-8f0f87b9f7d6`). This is the subnet of the worker nodes.

.. figure:: /_static/images/service-catalogs/openshift_machinset2.png
  :width: 600

  Figure 22. Copy subnet_id

3. Create the file :code:`machineset-example-eu-de-01.yaml` with the copied values:

.. code-block:: yaml

    # machineset-example-eu-de-01.yaml
    apiVersion: machine.openshift.io/v1beta1
    kind: MachineSet
    metadata:
      labels:
        machine.openshift.io/cluster-api-cluster: openshift-vts4p # <infra_id>, see Figure 21 above
        machine.openshift.io/cluster-api-machine-role: worker # <role>
        machine.openshift.io/cluster-api-machine-type: worker # <role>
      name: openshift-vts4p-worker # <infra_id>-<role>
      namespace: openshift-machine-api
    spec:
      replicas: 1 # <number_of_replicas>
      selector:
        matchLabels:
          machine.openshift.io/cluster-api-cluster: openshift-vts4p # <infra_id>
          machine.openshift.io/cluster-api-machineset: openshift-vts4p-worker # <infra_id>-<role>
      template:
        metadata:
          labels:
            machine.openshift.io/cluster-api-cluster: openshift-vts4p # <infra_id>
            machine.openshift.io/cluster-api-machine-role: worker # <role>
            machine.openshift.io/cluster-api-machine-type: worker # <role>
            machine.openshift.io/cluster-api-machineset: openshift-vts4p-worker # <infra_id>-<role>
        spec:
          providerSpec:
            value:
              apiVersion: machine.openshift.io/v1alpha1
              cloudName: openstack
              cloudsSecret:
                name: openstack-cloud-credentials
                namespace: openshift-machine-api
              flavor: s3.xlarge.4 # Choose a flavor from OTC with minimum 4 vCPU and 16GB RAM
              image: ""
              kind: OpenstackProviderSpec
              networks:
              - filter: {}
                subnets:
                - filter:
                    id: 1423eb8d-c552-410b-9b10-8f0f87b9f7d6 # <subnet_id>, see Figure 22 above
              rootVolume:
                diskSize: 120 # minimum 100
                sourceUUID: rhcos-418.94.202501221327-0-openstack.x86_64 # <cluster_os_image>, see Figure 21 above
                volumeType: SSD
              securityGroups:
              - filter: {}
                name: sg-worker
              trunk: false
              userDataSecret:
                name: worker-user-data
              availabilityZone: eu-de-01 # Choose 'eu-de-01', 'eu-de-02', and 'eu-de-03' (for OTC), 'eu-ch2a' and 'eu-ch2b' (for Swiss OTC).

4. and apply

.. code-block:: bash

    $ oc create -f machineset-example-eu-de-01.yaml

6.3 Expected result
-------------------

The requested worker node is up and running:

.. code-block:: bash

    $ oc get machines -n openshift-machine-api
    NAME                           PHASE     TYPE          REGION   ZONE      AGE
    openshift-vts4p-worker-nmwls   Running   s3.xlarge.4            eu-de-01  28m

7. Tear down
============

* In Cloud Create, go to **Action** / **Undeploy** to delete the OpenShift cluster.
* The PVC storages, which were created by OpenShift, will not be deleted automatically. You have to delete them manually.

.. figure:: /_static/images/service-catalogs/openshift_tear_down.png

  Figure 23. Check PVC with Available status

8. Links
========

* Our `OpenShift template in TOSCA <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs/blob/main/templates/openshift/4.13/topology.yml>`_.
* How to create a `PersistentVolume using NFS in OpenShift <https://docs.openshift.com/container-platform/4.13/storage/persistent_storage/persistent-storage-nfs.html>`_.
* `Maintaining credentials in OpenShift <https://docs.openshift.com/container-platform/4.14/post_installation_configuration/changing-cloud-credentials-configuration.html#manually-rotating-cloud-creds_changing-cloud-credentials-configuration>`_.