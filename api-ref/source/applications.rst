***********
Application
***********

1. Create a new application
===========================

This endpoint creates a new application (optionally from a template).

========  ===========================
Method    Path
========  ===========================
POST      /rest/latest/applications
========  ===========================

Parameters
----------

Request Body Parameters

===========================  ====================  =================================================================
Paramters                    Type                  Description
===========================  ====================  =================================================================
name                         string: <required>    Name of the application
description                  string: <optional>    Description of the application
topologyTemplateVersionId    string: <optional>    Template identifier that will be used to create the application
===========================  ====================  =================================================================

Samples
-------

Sample Payload

.. code-block::

  {
    "name": "app"
  }

Sample Request

.. code-block:: bash

  curl \
      --header "Content-Type: application/JSON" \
      --header "Accept: application/JSON" \
      --header "X-Auth-Token: $OSTOKEN" \
      --request POST \
      --data @payload.JSON \
      https://designer.otc-service.com/rest/latest/applications

Sample Response

.. code-block::

  {
    # the application identifier is returned in data field
    "data": "domain_name_project_name_app_1",
    "error": null
  }

2. Search for applications
==========================

This endpoint searches applications matching the request.

========  ==================================
Method    Path
========  ==================================
POST      /rest/latest/applications/search
========  ==================================

Parameters
----------

Request Body Parameters

===========  ===============================  =========================================================================================================
Paramters    Type                             Description
===========  ===============================  =========================================================================================================
filter       key-value-map: {}: <optional>    Filters that will be used in the request
from         integer: <optional>              Initial element index for the request. If null the value will be set to 0
query        string: <optional>               Query text
size         integer: <optional>              Maximum number of elements to return in the request. If null will be set to 50. Cannot be more than 100
===========  ===============================  =========================================================================================================

Samples
-------

Sample Payload

.. code-block::

  {
    "from": 0,
    "size": 20
  }

Sample Request

.. code-block:: bash

  curl \
      --header "Content-Type: application/JSON" \
      --header "Accept: application/JSON" \
      --header "X-Auth-Token: $OSTOKEN"  \
      --request POST \
      --data @payload.JSON \
      https://designer.otc-service.com/rest/latest/applications/search

Sample Response

.. code-block::

  {
    "data":
    {
      "types": ["Application"],
      "data": [
      {
        "id": "domain_name_project_name_app_1",
      "name": "app",
      "creationDate": 1600099973747,
      "lastUpdateDate": 1600099973976,
      "tags": [],
      "metaProperties": {},
      "userRoles":
      {
        "domain_name_user": ["APPLICATION_MANAGER"]
      },
      "projectId": "11111"
      },
      {
        "id": "domain_name_project_name_app_2",
        "name": "app2",
        "creationDate": 1600092341675,
        "lastUpdateDate": 1600092341890,
        "tags": [],
        "metaProperties": {},
        "userRoles":
        {
          "domain_name_user": ["APPLICATION_MANAGER"]
        },
        "projectId": "11111"
      }],
    "queryDuration": 4,
    "totalResults": 2,
    "from": 0,
    "to": 20,
    "facets": null
    },
    "error": null
  }

3. Delete an application
========================

This endpoint deletes an application by its identifier

========  ===========================================
Method    Path
========  ===========================================
DELETE    /rest/latest/applications/{applicationId}
========  ===========================================

Parameters
----------

Path Parameters

===============  ====================  ===============================
Paramters        Type                  Description
===============  ====================  ===============================
applicationId    string: <required>    Identifier of the application
===============  ====================  ===============================

Samples
-------

Sample Request

.. code-block:: bash

  curl \
      --header "Content-Type: application/JSON" \
      --header "Accept: application/JSON" \
      --header "X-Auth-Token: $OSTOKEN" \
      --request DELETE \
      https://designer.otc-service.com/rest/latest/applications/domain_name_project_name_app

Sample Response

.. code-block::

  {
    "data": true,
    "error": null
  }

4. Get an application by its id
===============================

This endpoint returns the application details.

========  ===========================================
Method    Path
========  ===========================================
GET       /rest/latest/applications/{applicationId}
========  ===========================================

Parameters
----------

Path Parameters

===============  ====================  ===============================
Paramters        Type                  Description
===============  ====================  ===============================
applicationId    string: <required>    Identifier of the application
===============  ====================  ===============================

Samples
-------

Sample Request

.. code-block:: bash

  curl \
      --header "Content-Type: application/JSON" \
      --header "Accept: application/JSON" \
      --header "X-Auth-Token: $OSTOKEN" \
      --request GET  \
      https://designer.otc-service.com/rest/latest/applications/domain_name_project_name_app_1

Sample Response

.. code-block::

  {
    "data":
    {
      "id": "domain_name_project_name_app_1",
    "name": "app",
    "creationDate": 1600099973747,
    "lastUpdateDate": 1600099973747,
    "tags": [],
    "metaProperties": {},
    "userRoles":
    {
      "domain_name_user_name": ["APPLICATION_MANAGER"]
    },
    "projectId": "222222"
    }
  }

5. Update application
=====================

This endpoint updates name or description of the application

========  ===========================================
Method    Path
========  ===========================================
PUT       /rest/latest/applications/{applicationId}
========  ===========================================

Parameters
----------

Path Parameters

===============  ====================  ===============================
Paramters        Type                  Description
===============  ====================  ===============================
applicationId    string: <required>    Identifier of the application
===============  ====================  ===============================

Request Body Parameters

=============  ====================  ================================
Paramters      Type                  Description
=============  ====================  ================================
description    string: <optional>    Description of the application
name           string: <optional>    Name of the application
=============  ====================  ================================

Samples
-------

Sample Payload

.. code-block::

  {
    "description": "foo",
    "name": "bar"
  }

Sample Request

.. code-block:: bash

  curl \
      --header "Content-Type: application/JSON" \
      --header "Accept: application/JSON" \
      --header "X-Auth-Token: $OSTOKEN" \
      --request PUT \
      --data @payload.JSON \
      https://designer.otc-service.com/rest/latest/applications/domain_name_project_name_app_1

Sample Response

.. code-block::

  {
    "data": null,
    "error": null
  }

6. Search for application environments
======================================

This endpoint returns a search result with that contains application environments DTO matching the request.

========  ===============================================================
Method    Path
========  ===============================================================
POST      /rest/latest/applications/{applicationId}/environments/search
========  ===============================================================

Parameters
----------

Path Parameters

===============  ====================  ===============================
Paramters        Type                  Description
===============  ====================  ===============================
applicationId    string: <required>    Identifier of the application
===============  ====================  ===============================

Request Body Parameters

===========  ===============================  =========================================================================================================
Paramters    Type                             Description
===========  ===============================  =========================================================================================================
filter       key-value-map: {}: <optional>    Filters that will be used in the request
from         integer: <optional>              Initial element index for the request. If null the value will be set to 0
query        string: <optional>               Query text
size         integer: <optional>              Maximum number of elements to return in the request. If null will be set to 50. Cannot be more than 100
===========  ===============================  =========================================================================================================

Samples
-------

Sample Payload

.. code-block::

  {
    "from": 0,
    "size": 20
  }

Sample Request

.. code-block:: bash

  curl \
      --header "Content-Type: application/JSON" \
      --header "Accept: application/JSON" \
      --header "X-Auth-Token: $OSTOKEN" \
      --request POST \
      --data @payload.JSON \
      https://designer.otc-service.com/rest/latest/applications/domain_name_project_name_App1/environments/search

Sample Response

.. code-block::

  {
    "data": {
      "types": [
        "applicationenvironment"
      ],
      "data": [
        {
          "id": "a6f4d52d-6gh7-4vf3-1230-2fgvb3dr5d52",
          "status": "INIT_DEPLOYMENT",
          "name": "Environment",
          "applicationId": "domain_name_project_name_App1",
          "environmentType": "DEVELOPMENT",
          "currentVersionName": "0.1.0-SNAPSHOT",
          "deployedVersion": "0.1.0-SNAPSHOT",
          "userRoles": {
            "domain_name_user_name": [
              "DEPLOYMENT_MANAGER"
            ]
          }
        }
      ],
      "queryDuration": 1,
      "totalResults": 1,
      "from": 0,
      "to": 0
    },
    "error": null
  }

7. Search for topologies in the catalog
=======================================

========  ========================================
Method    Path
========  ========================================
POST      /rest/latest/catalog/topologies/search
========  ========================================

Parameters
----------

Request Body Parameters

===========  ===============================  =========================================================================================================
Paramters    Type                             Description
===========  ===============================  =========================================================================================================
filter       key-value-map: {}: <optional>    Filters that will be used in the request
from         integer: <optional>              Initial element index for the request. If null the value will be set to 0
query        string: <optional>               Query text
size         integer: <optional>              Maximum number of elements to return in the request. If null will be set to 50. Cannot be more than 100
===========  ===============================  =========================================================================================================

Samples
-------

Sample Payload

.. code-block::

  {
    "from": 0,
    "size": 20
  }

Sample Request

.. code-block:: bash

  curl \
      --header "Content-Type: application/JSON" \
      --header "Accept: application/JSON" \
      --header "X-Auth-Token: $OSTOKEN" \
      --request POST \
      --data @payload.JSON \
      https://designer.otc-service.com/rest/latest/catalog/topologies/search

Sample Response

.. code-block::

  {
    "data": {
      "types": [
        "topology",
        "topology"
      ],
      "data": [
        {
          "archiveName": "1-HelloWorld",
          "archiveVersion": "1.0.0",
          "nestedVersion": {
            "majorVersion": 1,
            "minorVersion": 0,
            "incrementalVersion": 0,
            "buildNumber": 0
          },
          "workspace": "ALIEN_GLOBAL_WORKSPACE",
          "description": "Description",
          "creationDate": 1602830832206,
          "lastUpdateDate": 1602830832206,
          "dependencies": [],
          "unprocessedWorkflows": {},
          "empty": true,
          "id": "1-HelloWorld:1.0.0"
        },
        {
          "archiveName": "4-NodejsWebApp",
          "archiveVersion": "1.0.0",
          "nestedVersion": {
            "majorVersion": 1,
            "minorVersion": 0,
            "incrementalVersion": 0,
            "buildNumber": 0
          },
          "workspace": "ALIEN_GLOBAL_WORKSPACE",
          "description": "Description",
          "creationDate": 1602830833111,
          "lastUpdateDate": 1602830833111,
          "dependencies": [],
          "unprocessedWorkflows": {},
          "empty": true,
          "id": "4-NodejsWebApp:1.0.0"
        }
      ],
      "queryDuration": 0,
      "totalResults": 2,
      "from": 0,
      "to": 2,
      "facets": {}
    },
    "error": null
  }