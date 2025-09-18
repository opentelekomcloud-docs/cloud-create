********
Examples
********

.. important:: This API will be released in Cloud Create v2.20.

1. Example to quick deploy OpenShift on Swiss OTC
=================================================

Step 1. (Optional) Show the template
------------------------------------

Request

.. code-block:: bash

    curl \
    --header "Accept: application/json" \
    --header "X-Auth-Token: $OSTOKEN" \
    --header "X-Auth-Location: OTC-EU-CH2" \ # Add this header for Swiss OTC
    --request GET \
    https://designer.otc-service.com/api/v2/templates/OpenShift:4.16.19

Response

.. code-block:: json

    {
      "data": {
        "id": "OpenShift:4.16.19",
        "template_name": "OpenShift",
        "template_version": "4.16.19",
        "template_author": "Dr. Tri Vo",
        "public_template": true,
        "documentation_link": "https://docs.otc.t-systems.com/cloud-create/umn/service_catalogs/openshift.html",
        "description": "This template deploys a Self-managed OpenShift Container Platform on Open Telekom Cloud with worker nodes in one availability zone...",
        "creation_date": "2024-11-27T14:56:00.353+00:00",
        "last_update_date": "2024-11-27T14:56:00.353+00:00",
        "inputs": {
          "ssh_public_key": {
            "type": "string",
            "required": false,
            "description": "Specify the SSH public key so that you can SSH to the Bastionhost, OpenShift control, and worker nodes after the deployment...",
            "default": null
          },
          "base_domain": {
            "type": "string",
            "required": true,
            "description": "Specify the base domain for the OpenShift cluster <example.com>...",
            "default": null
          },
          "cluster_name": {
            "type": "string",
            "required": true,
            "description": "Specify the cluster name <openshift>",
            "default": "openshift"
          },
          "number_workers": {
            "type": "integer",
            "required": true,
            "description": "Specify how many worker nodes to create. OpenShift requires minimum 2 worker nodes to setup all operators...",
            "default": "2"
          },
          "nat_gateway_spec": {
            "type": "string",
            "required": true,
            "description": "The specification of the NAT Gateway. 'Micro' supports up to 1,000 SNAT connections. 'Small' supports up to 10,000 SNAT connections...",
            "default": "Small"
          },
          "worker_num_cpus": {
            "type": "integer",
            "required": false,
            "description": "Number of CPUs associated with the worker node. OpenShift requires minimum 4 vCPUs for worker node.",
            "default": "4"
          },
          "worker_mem_size": {
            "type": "scalar-unit.size",
            "required": false,
            "description": "Size of memory associated with the worker node. OpenShift requires minimum 16 GB for worker node.",
            "default": "16 GB"
          },
          "worker_availability_zone": {
            "type": "string",
            "required": false,
            "description": "Choose the availability zone for the worker nodes. The value 'az-*' is equivalent to 'eu-de-*' for the deployment in region Germany, and 'eu-ch2-*' for the deployment in Switzerland.",
            "default": "az-01",
          }
        },
        "secrets": {
          "pull_secret": {
            "type": "string",
            "required": true,
            "description": "Specify the pull secret."
          },
          "os_password": {
            "type": "string",
            "required": true,
            "description": "Specify the password to authenticate to the API keystone."
          }
        },
        "outputs": {
          "OpenShiftInstall_infra_id": {
            "description": ""
          },
          "OpenShiftInstall_console_hostname": {
            "description": ""
          },
          "OpenShiftInstall_kubeadmin_username": {
            "description": "The default username to login to OpenShift console for administration."
          },
          "Bastionhost_public_address": {
            "description": "The primary public IP address assigned by the cloud provider that applications may use to access the Compute node."
          },
          "OpenShiftInstall_kubeadmin_password": {
            "description": ""
          },
          "INGRESS_VIP_public_address": {
            "description": "The floating ip address assigned to this VIP, if it connects to a public network."
          },
          "OpenShiftInstall_info": {
            "description": "Useful information to display to the users after the deployment completes."
          },
          "OpenShiftInstall_console_url": {
            "description": ""
          },
          "Workers_private_address": {
            "description": "The primary private IP address assigned by the cloud provider that applications may use to access the Compute node."
          },
          "OpenShiftInstall_oauth_hostname": {
            "description": ""
          }
        }
      },
      "error": null
    }

Step 2. Prepare the required inputs
-----------------------------------

1. Copy the pull secret to a file :code:`pull_secret.json`

.. code-block:: bash

    $ vim pull_secret.json
    # Paste the pull_secret text
    {"auths":{"cloud.openshift.com":{ ... }}}

2. Prepare the POST request body in a file :code:`payload.json`

.. code-block:: bash

    # The secret pull_secret is a text with double quotes so we use jq to automatically escapes quotes
    jq -n \
      --arg os_password "$OS_PASSWORD" \
      --arg pull_secret "$(cat pull_secret.json)" \
      '{ template_id: "OpenShift:4.16.19",
         inputs: { base_domain: "tri-test.com" },
         secrets: { os_password: $os_password, pull_secret: $pull_secret } }' \
      > payload.json

Now you have the :code:`payload.json`

.. code-block:: json

    {
      "template_id": "OpenShift:4.16.19",
      "inputs": {
        "base_domain": "tri-test.com"
      },
      "secrets": {
        "os_password": "<your password>",
        "pull_secret": "{\"auths\":{\"cloud.openshift.com\":{ ... }}}" // double quotes are now escaped
      }
    }

Step 3. Deploy the OpenShift template with the required inputs
--------------------------------------------------------------

Request

.. code-block:: bash

    curl \
    --header "Content-Type: application/JSON" \
    --header "Accept: application/JSON" \
    --header "X-Auth-Token: $OSTOKEN" \
    --header "X-Auth-Location: OTC-EU-CH2" \ # Add this header for Swiss OTC
    --request POST \
    --data @payload.json \
    https://designer.otc-service.com/api/v2/deployments

Response

.. code-block:: bash

    HTTP/1.1 201 Created
    Location: /rest/v2/deployments/c853667c-2901-4274-aaec-f747e6649cdd
    Content-Type: application/json
    {
      "data": {
        "id": "c853667c-2901-4274-aaec-f747e6649cdd"
      },
      "error": null
    }

Step 4. Get deployment status
-----------------------------

Request

.. code-block:: bash

    curl \
    --header "Accept: application/json" \
    --header "X-Auth-Token: $OSTOKEN" \
    --header "X-Auth-Location: OTC-EU-CH2" \ # Add this header for Swiss OTC
    --request GET \
    https://designer.otc-service.com/api/v2/deployments/c853667c-2901-4274-aaec-f747e6649cdd

Response

.. code-block:: json

    {
      "data": {
        "id": "c853667c-2901-4274-aaec-f747e6649cdd",
        "deployer_username": "username",
        "location_name": "OTC-EU-CH2",
        "application_id": "cdf1988e554e4c9f9074d12f0ef12958",
        "application_name": "OpenShift00", // A new application OpenShift00 was created from the template
        "application_version": "0.1.0-SNAPSHOT",
        "environment_id": "d458fd8d-8618-4050-b113-38c85f00a808",
        "start_date": "2025-07-10T12:02:35.838+00:00",
        "deyployment_status": "DEPLOYMENT_IN_PROGRESS" // DEPLOYED, UNDEPLOYED
      },
      "error": null
    }