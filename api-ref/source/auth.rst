******************
API Authentication
******************

Before sending a REST request to the API, you get an `OSTOKEN` as follows:

1. Get OSTOKEN
==============

Install OpenStack client (if you don't have it)

.. code-block:: bash

  # To install Openstack client on Ubuntu
  sudo apt-get install python3-openstackclient

Create the following file **openrc**:

.. code-block:: bash

  #For OTC, use this IAM endpoint:
  export OS_AUTH_URL=https://iam.eu-de.otc.t-systems.com:443/v3

  #For Swiss OTC, use this IAM endpoint:
  #export OS_AUTH_URL=https://iam-pub.eu-ch2.sc.otc.t-systems.com/v3

  export OS_IDENTITY_API_VERSION=3
  export OS_USER_DOMAIN_NAME=your_user_domain_name
  export OS_PROJECT_NAME=your_project_name
  export OS_USERNAME=your_username
  export OS_PASSWORD=your_password

Source the file

.. code-block:: bash

  source openrc

Get the OSTOKEN

.. code-block:: bash

  OSTOKEN=$(openstack token issue -c "id" -f value)

2. Send REST requests
=====================

For all REST requests,  set the `OSTOKEN` in the HTTP header `X-Auth-Token` to authenticate it. For examples:

2.1. Requests for OTC
^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

  curl \
    --header "Accept: application/json" \
    --header "X-Auth-Token: $OSTOKEN" \
    --request GET \
    https://designer.otc-service.com/api/v2/templates

2.2. Requests for Swiss OTC
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

  curl \
    --header "X-Auth-Token: $OSTOKEN" \
    --header "X-Auth-Location: OTC-EU-CH2" \ # Add this header for Swiss OTC
    --request GET \
    https://designer.otc-service.com/api/v2/templates

.. important::
  **TTL token limit**: The OSTOKEN issued by Open Telekom Cloud has a default TTL of 24 hours. However, Cloud Create only accepts the OSTOKEN with the TTL of 30 minutes to limit a stolen token. After 30 minutes, you should get a new one.