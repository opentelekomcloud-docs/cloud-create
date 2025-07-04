.. _argocd:

****************
Argo CD template
****************

.. note::

  To be released in v2.19

1. About
========

.. figure:: /_static/images/service-catalogs/argocd.png
  :width: 900

**GitOps-Driven App Deployment on CCE—Powered by Argo CD and Cloud Create**

With Cloud Create, deploying your applications to the cloud has never been easier—or faster. In just a few clicks, you can spin up a Cloud Container Engine (CCE) cluster and launch your apps using Argo CD, the leading GitOps tool.

Just point to your Git repository containing your Kubernetes manifests, and Cloud Create takes care of the rest. Argo CD automatically pulls your K8s resources and syncs them to a fresh CCE cluster. Once deployment is complete, your apps are live—fully automated, fully operational.

Need to scale or add more apps later? Simply log into Argo CD and deploy directly from your Git repo—no manual YAML updates, no downtime.

**Cloud-native deployment, GitOps simplicity, zero hassle**.

.. figure:: /_static/images/service-catalogs/argocd0.png
  :width: 900

  Figure 1. The Argo CD template

2. How to use
=============

2.1. Enable CCE on the Web console (on first use)
-------------------------------------------------

If this is the first time you use CCE **in a project**, you have to authorize it first.

1. Login to the Web console of `OTC <https://console.otc.t-systems.com>`_ / or `Swiss OTC <https://console.sc.otc.t-systems.com>`_.
2. Switch to the project you want to deploy CCE / go to **Cloud Container Engine**.
3. The Web console shows an **Authorization Description** > Click OK.

.. figure:: /_static/images/service-catalogs/cce_enable.png
  :width: 700

  Figure 2. Accept the Authoriztation on first use

2.2. Deploy Setup
-----------------

The following tutorial uses the example app "sock-shop" from Argo CD to demonstrate:

1. Input **app_name** with a name of your app (e.g., :code:`sock-shop`).
2. Input **repo** with the Git repository where you want Argo CD to sync (e.g., :code:`https://github.com/argoproj/argocd-example-apps.git`).
3. Input **path** with the path on the Git repository, which contains the K8s resources you want to deploy (e.g., :code:`sock-shop`).
4. (Optional) Enable the option **access_with_elb**, if you wish to access Argo CD via a public IP address.

.. figure:: /_static/images/service-catalogs/argocd1.png
  :width: 700

  Figure 3. Argo CD setup

2.3. Access Argo CD
-------------------

2.3.1. Access Argo CD with ELB IP
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* If you enable **access_with_elb** in the Deploy Setup, the deployment outputs the public IP address of Argo CD **elb_ip** (see Figure 4, nr.1).

.. figure:: /_static/images/service-catalogs/argocd2.png
  :width: 700

  Figure 4. Outputs of Argo CD

* Copy the **elb_ip** in a browser. Use the username **admin** and the initial admin password to access Argo CD (see Figure 4, nr.2).

.. figure:: /_static/images/service-catalogs/argocd3.png
  :width: 700

  Figure 5. Login with admin

.. important::

  Remember to change the initial password after login. This password is displayed in plaintext.

* Inside Argo CD, you can check the status of your app or add more apps if needed:

.. figure:: /_static/images/service-catalogs/argocd4.png
  :width: 700

  Figure 6. The app :code:`sock-shop` is synced

2.3.2. Access Argo CD via port forwarding
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you do not wish to expose Argo CD to public access, disable **access_with_elb** in the Deploy Setup. In this case, you can access it via the bastion host as follows:

1. Enable TCP forwarding on the bastion host

.. code-block:: bash

    # SSH to the bastion host
    $ ssh ubuntu@<bastion_host_ip>

    # Enable TCP forwarding in the sshd_config
    $ sudo nano /etc/ssh/sshd_config
    # update sshd_config with the following values
    AllowTcpForwarding yes
    PermitOpen any

    # Restart SSH
    $ sudo systemctl restart ssh

2. Start SSH Port Forwarding

.. code-block:: bash

    # On your local machine, forwards connections from local port 3000 to port 3000 on the bastion host
    $ ssh -L 3000:localhost:3000 ubuntu@<bastion_host_ip>

    # On the bastion host, start port forwarding
    $ kubectl port-forward service/argocd-server -n argocd 3000:443

4. Argo CD can then be accessed from your local machine with :code:`https://localhost:3000`.

.. figure:: /_static/images/service-catalogs/argocd5.png
  :width: 700

  Figure 7. Access :code:`https://localhost:3000`

2.4. Access the example app
---------------------------

The example app :code:`sock-shop` is up and running. To access it with a public IP address, you need to create a service from type Load Balancer as follows:

1. Access the bastion host

.. code-block:: bash

    # SSH to the bastion host
    $ ssh ubuntu@<bastion_host_ip>

2. Create a file "svc-front-end.yaml" with the following content:

.. code-block:: yaml

    # Create a service with the type Load Balancer and auto-create an EIP
    ---
    kind: Service
    apiVersion: v1
    metadata:
      name: front-end-elb
      annotations:
        service.protal.kubernetes.io/type: LoadBalancer
        kubernetes.io/elb.class: union
        kubernetes.io/elb.autocreate: '{"type":"public","bandwidth_name":"sock-shop-bandwith","bandwidth_chargemode":"traffic","bandwidth_size":5,"bandwidth_sharetype":"PER","eip_type":"5_bgp"}'
    spec:
      selector:
        name: front-end
      ports:
        - name: front-end
          protocol: TCP
          port: 80
          targetPort: 8079
      type: LoadBalancer
      loadBalancerIP: ''

and apply

.. code-block:: bash

    $ kubectl apply -f svc-front-end.yaml

5. Get the EXTERNAL-IP of the new service:

.. code-block:: bash

    # The EXTERNAL-IP is 80.158.3.11
    $ kubectl get svc front-end-elb
    NAME            TYPE           CLUSTER-IP       EXTERNAL-IP              PORT(S)        AGE
    front-end-elb   LoadBalancer   10.247.230.212   10.0.0.115,80.158.3.11   80:32159/TCP   11s

and access it with a browser:

.. figure:: /_static/images/service-catalogs/argocd6.png
  :width: 700

  Figure 8. Access :code:`http://80.158.3.11`

2.3. Customize Argo CD with helm chart
--------------------------------------

By default, we use `the community maintained helm chart <https://argoproj.github.io/argo-helm>`_ to deploy Argo CD. You can customize the helm chart with your values:

Document to be continued...

.. tip::

  * See the :ref:`cce` template on how to customize the CCE.

3. Links
========

* Our `Argo CD template in TOSCA <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs/blob/main/templates/argocd/topology.yml>`_.
* The `community maintained helm chart of Argo CD <https://argoproj.github.io/argo-helm>`_.