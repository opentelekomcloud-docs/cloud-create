******************
API Authentication
******************

Before sending a REST request to the API, you get an `OSTOKEN` as follows:

1. Get OSTOKEN
==============

Create the following file **openrc**:

.. code-block:: bash

  export OS_AUTH_URL=https://iam.eu-de.otc.t-systems.com:443/v3
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

For each REST request,  set the `OSTOKEN` in the HTTP header `X-Auth-Token` to authenticate it:

.. code-block:: bash

  curl \
    --header "Content-Type: application/json" \
    --header "Accept: application/json" \
    --header "X-Auth-Token: $OSTOKEN"
    ...

.. note::
  **TTL token limit**: The OSTOKEN issued by Open Telekom Cloud has a default TTL of 24 hours. However, Cloud Create only accept the OSTOKEN with the TTL of 30 minutes to limit a stolen token.