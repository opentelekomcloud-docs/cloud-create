**********
Deployment
**********

1. Deploy an application
========================

This endpoint deploys an application for the given environment.

========  ======================================
Method    Path
========  ======================================
POST      /rest/latest/applications/deployment
========  ======================================

Parameters
==========

==========================  ====================  ===========================================
Request Body Parameters
==========================  ====================  ===========================================
applicationEnvironmentId    string: <required>    Identifier of the application environment
applicationId               string: <required>    Identifier of the application
==========================  ====================  ===========================================

Samples
-------

Sample Payload

.. code-block::

  {
    "applicationId":"domain_name_project_name_app1",
    "applicationEnvironmentId":"d4gh16d4-y66e-48he-34a9-d0e1df4576a0"
  }

Sample Request

.. code-block:: bash

  curl \
      --header "Content-Type: application/json" \
      --header "Accept: application/json" \
      --header "X-Auth-Token: $OSTOKEN" \
      --request POST \
      --data @payload.json \
      https://designer.otc-service.com/rest/latest/applications/deployment

Sample Response

.. code-block::

  {
    "data": null,
    "error": null
  }

2. Un-Deploy an application
===========================

This endpoint undeploys an application from the given environment.

========  ==============================================================================================
Method    Path
========  ==============================================================================================
PUT       /rest/latest/applications/{applicationId}/environments/{applicationEnvironmentId}/deployment
========  ==============================================================================================

Parameters
==========

Path Parameters

==========================  =====================  ===========================================
Paramters                   Type                   Description
==========================  =====================  ===========================================
applicationEnvironmentId    string: <required>     Identifier of the application environment
applicationId               string: <required>     Identifier of the application
force                       boolean: <optional>    Whether the undeployment is forced
==========================  =====================  ===========================================

Samples
-------

Sample Payload

.. code-block::

  {}

Sample Request

.. code-block:: bash

  curl \
      --header "Content-Type: application/json" \
      --header "Accept: application/json" \
       --header "X-Auth-Token: $OSTOKEN" \
      --request PUT \
      --data @payload.json \
      https://designer.otc-service.com/rest/latest/applications/domain_name_project_name_app1/environments/854h523f-1b4c-4h17-9g52-42hb4u88db6c/deployment?force=false

Sample Response

.. code-block::

  {
    "data": null,
    "error": null
  }

3. Search for deployments
=========================

This endpoint searches for deployments matching the request.

========  ===========================================================================
Method    Path
========  ===========================================================================
GET       /rest/latest/deployments/search?environmentId=envId&from=0&query=&size=15
========  ===========================================================================

Parameters
==========

Path Parameters

======================  =====================  ===============================================================================================================================
Paramters               Type                   Description
======================  =====================  ===============================================================================================================================
query                   string: <optional>     Query text
from                    integer: <optional>    Query from the given index
size                    integer: <optional>    Maximum number of results to retrieve
orchestratorId          string: <optional>     Identifier of the orchestrator for which to get deployments. If not provided, get deployments for all orchestrators
sourceId                string: <optional>     Identifier of the application for which to get deployments. If not provided, get deployments for all applications
environmentId           string: <optional>     Identifier of the environment for which to get deployments. If not provided, get deployments without filtering by environment
includeSourceSummary    boolean: <optional>    Include or not the source (application or csar) summary in the results
======================  =====================  ===============================================================================================================================

Samples
-------

Sample Request

.. code-block:: bash

  curl \
      --header "Content-Type: application/json" \
      --header "Accept: application/json" \
      --header "X-Auth-Token: $OSTOKEN" \
      --request GET \
      https://designer.otc-service.com/rest/latest/deployments/search?environmentId=854h523f-1b4c-4h17-9g52-42hb4u88db6c&from=0&query=&size=15

Sample Response

.. code-block::

  {
    "data":
    {
      "types":["deployment","deployment"],
      "data":[
      {
        "deployment":
        {
          "id":"05ckj5a1-dge4-40f3-a328-32ehfnr58c88",
          "orchestratorDeploymentId":"domain_name_project_name_app1-Environment",
          "deployerUsername":"domain_name_user_name",
          "sourceType":"APPLICATION",
          "orchestratorId":"18hj38a8-9c91-4469-83b9-1adjn32hta42",
          "locationIds":["9021a3e2-72c8-4869-8e8daa7266d4787d"],
          "sourceId":"domain_name_project_name_app1",
          "sourceName":"app1",
          "environmentId":"8jd3243f-150c-4j77-9b52-427790hg5b6c",
          "versionId":"0.1.0-SNAPSHOT",
          "startDate":1601383741469,
          "endDate":1601383882433,
          "workflowExecutions":{}
        },
        "source":
        {
          "id":"domain_name_project_name_app1",
          "name":"app1"
        },
        "locations":[
        {
          "groupPermissions":
          {
            "eb44bbfc-a3e4-4cb6-87e8-1bba7b3d7201":["ADMIN"]
          },
          "id":"9021a3e2-72c8-4869-8e8d-aa7266d4787d",
          "name":"OTC",
          "orchestratorId":"18d568a8-9c91-4469-83b9-1adc1332ca42",
          "dependencies":[],
          "lastUpdateDate":1601384719495
        }]
      },
      {
        "deployment":
        {
          "id":"0dghg4ed-3aec-42a6-a50e-b1nbh94bd8e7",
          "orchestratorDeploymentId":"domain_name_project_name_app1-Environment",
          "deployerUsername":"domain_name_user_name",
          "sourceType":"APPLICATION",
          "orchestratorId":"18hj38a8-9c91-4469-83b9-1adjn32hta42",
          "locationIds":["9034vee2-72c8-4869-8e8d-aa7266d4787d"],
          "sourceId":"domain_name_project_name_app1",
          "sourceName":"app1",
          "environmentId":"8jd3243f-150c-4j77-9b52-427790hg5b6c",
          "versionId":"0.1.0-SNAPSHOT",
          "startDate":1601383371481,
          "endDate":1601383589403,
          "workflowExecutions":{}
        },
        "source":
        {
          "id":"domain_name_project_name_app1",
          "name":"app1"
        },
        "locations":[
        {
          "groupPermissions":
          {
            "eb44bbfc-a3e4-4cb6-87e8-1bba7b3d7201":["ADMIN"]
          },
          "id":"9021a3e2-72c8-4869-8e8d-aa7266d4787d",
          "name":"OTC",
          "orchestratorId":"18d568a8-9c91-4469-83b9-1adc1332ca42",
          "dependencies":[],
          "lastUpdateDate":1601384719495
        }]
      }],
      "queryDuration":0,
      "totalResults":2,
      "from":0,"to":1,
      "facets":null
    },
    "error":null
  }

4. Set location policies for a deployment
=========================================

This endpoint set location policies for a deployment.
Creates if not yet the DeploymentTopology object linked to this deployment.

========  ======================================================================================================
Method    Path
========  ======================================================================================================
POST      /rest/latest/applications/{appId}/environments/{environmentId}/deployment-topology/location-policies
========  ======================================================================================================

Parameters
==========

Path Parameters

===============  ====================  =====================================================================
Paramters        Type                  Description
===============  ====================  =====================================================================
appId            string: <required>    Identifier of the application
environmentId    string: <required>    Identifier of the environment on which to set the location policies
===============  ====================  =====================================================================

Request Body Parameters

===================  ===============================  ======================================================================================
Paramters            Type                             Description
===================  ===============================  ======================================================================================
groupsToLocations    key-value-map: {}: <required>    Locations settings for groups. Key = groupName, value = locationId.
orchestratorId       string: <required>               Identifier of the orchestratrator managing the locations on which we want to deploy.
===================  ===============================  ======================================================================================

Samples
-------

Sample Payload

.. code-block::

  {
    "orchestratorId":"4g5b6739-6j7d-4541-bn3c-ang56ht31998",
    "groupsToLocations":{
    "_A4C_ALL":"a6bnh54e-a0e1-4n2f-ahn3-87ng56gy4178"
    }
  }

Sample Request

.. code-block:: bash

  curl \
      --header "Content-Type: application/json" \
      --header "Accept: application/json" \
       --header "X-Auth-Token: $OSTOKEN" \
      --request POST \
      --data @payload.json \
      https://designer.otc-service.com/rest/latest/applications/domain_name_project_name_app1/environments/17bn5t2c-3bg3-4bn4-945e-8fbn45fbfe4/deployment-topology/location-policies

Response Schema

.. code-block::

  {
    "data": {
      "availableSubstitutions": {
        "availablePoliciesSubstitutions": {},
        "availableSubstitutions": {},
        "substitutionTypes": {
          "allNodeTypes": {},
          "capabilityTypes": {},
          "configurationTypes": {},
          "dataTypes": {},
          "nodeTypes": {},
          "onDemandTypes": {},
          "policyTypes": {},
          "providedTypes": [
            "string"
          ]
        },
        "substitutionsPoliciesTemplates": {},
        "substitutionsTemplates": {}
      },
      "capabilityTypes": {},
      "dataTypes": {},
      "locationPolicies": {},
      "locationResourceTemplates": {},
      "nodeTypes": {},
      "policyLocationResourceTemplates": {},
      "policyTypes": {},
      "relationshipTypes": {},
      "secretCredentialInfos": [
        {
          "credentialDescriptor": {},
          "pluginName": "string"
        }
      ],
      "topology": {
        "archiveName": "string",
        "archiveVersion": "string",
        "creationDate": "2020-10-01T08:16:15.986Z",
        "dependencies": [
          {
            "hash": "string",
            "name": "string",
            "version": "string"
          }
        ],
        "deployed": true,
        "deployerInputProperties": {},
        "description": "string",
        "empty": true,
        "environmentId": "string",
        "groups": {},
        "id": "string",
        "initialTopologyId": "string",
        "inputArtifacts": {},
        "inputs": {},
        "lastDeploymentTopologyUpdateDate": "2020-10-01T08:16:15.986Z",
        "lastUpdateDate": "2020-10-01T08:16:15.986Z",
        "locationDependencies": [
          {
            "hash": "string",
            "name": "string",
            "version": "string"
          }
        ],
        "locationGroups": {},
        "matchReplacedNodes": {},
        "metaProperties": {},
        "nestedVersion": {
          "buildNumber": 0,
          "incrementalVersion": 0,
          "majorVersion": 0,
          "minorVersion": 0,
          "qualifier": "string"
        },
        "nodeTemplates": {},
        "orchestratorId": "string",
        "originalNodes": {},
        "originalPolicies": {},
        "outputAttributes": {},
        "outputCapabilityProperties": {},
        "outputProperties": {},
        "policies": {},
        "preconfiguredInputProperties": {},
        "providerDeploymentProperties": {},
        "substitutedNodes": {},
        "substitutedPolicies": {},
        "substitutionMapping": {
          "capabilities": {},
          "requirements": {},
          "substitutionType": "string"
        },
        "tags": [
          {
            "name": "string",
            "value": "string"
          }
        ],
        "unprocessedWorkflows": {},
        "uploadedInputArtifacts": {},
        "versionId": "string",
        "workflows": {},
        "workspace": "string"
      },
      "validation": {
        "infoList": [
          {
            "code": "LOG",
            "source": "string"
          }
        ],
        "taskList": [
          {
            "code": "LOG",
            "source": "string"
          }
        ],
        "valid": true,
        "warningList": [
          {
            "code": "LOG",
            "source": "string"
          }
        ]
      }
    },
    "error": {
      "code": 0,
      "message": "string"
    }
  }

5. Get deployment status from its id
====================================

========  ================================================
Method    Path
========  ================================================
GET       /rest/latest/deployments/{deploymentId}/status
========  ================================================

Parameters
==========

Path Parameters

==============  ====================  ==============================================================
Paramters       Type                  Description
==============  ====================  ==============================================================
deploymentId    string: <required>    Identifier of the orchestrator for which to get deployments.
==============  ====================  ==============================================================

Samples
-------

Sample Request

.. code-block:: bash

  curl \
      --header "Content-Type: application/json" \
      --header "Accept: application/json" \
      --header "X-Auth-Token: $OSTOKEN" \
      --request GET \
      https://designer.otc-service.com/rest/latest/deployments/05cadea1-ddc3-40f3-a328-32e257618c88/status

Sample Response

.. code-block::

  {
    "data":"FAILURE",
    "error":null
  }

6. Search for last workflow execution monitor data
==================================================

This endpoint for a given deployment, get the last workflow execution monitor data.

========  ================================================
Method    Path
========  ================================================
GET       /rest/latest/workflow_execution/{deploymentId}
========  ================================================

Parameters
==========

Path Parameters

==============  ====================  ==============================
Paramters       Type                  Description
==============  ====================  ==============================
deploymentId    string: <required>    Identifier of the deployment
==============  ====================  ==============================

Samples
-------

Sample Request

.. code-block:: bash

  curl \
      --header "Content-Type: application/json" \
      --header "Accept: application/json" \
       --header "X-Auth-Token: $OSTOKEN" \
      --request GET \
      https://designer.otc-service.com/rest/latest/workflow_execution/e55f8112-0c0e-4645-ae92-05189399122f

Sample Response

.. code-block::

  {
    "data": {
      "execution": {
        "id": "7dvbfr44-9vb2-vfrd-vb43-gtr3fvgfa4b6",
        "deploymentId": "8bg45f51-2bn4-fg33-vf5d-vbgf34fd79b6",
        "workflowId": "install",
        "workflowName": "install",
        "displayWorkflowName": "install",
        "startDate": 1602742451105,
        "endDate": 1602742490619,
        "status": "FAILED",
        "hasFailedTasks": true
      },
      "actualKnownStepInstanceCount": 7,
      "lastKnownExecutingTask": null,
      "stepStatus": {
        "Vpc_test_3_install": "COMPLETED_SUCCESSFULL",
        "Secrule_inbound_BastionHost_install": "COMPLETED_SUCCESSFULL",
        "FIPPort_install": "COMPLETED_SUCCESSFULL",
        "Port_install": "COMPLETED_WITH_ERROR",
        "Port_1_install": "COMPLETED_SUCCESSFULL",
        "Secgroup_BastionHost_install": "COMPLETED_SUCCESSFULL",
        "Private_install": "COMPLETED_SUCCESSFULL"
      },
      "stepInstances": {
        "Vpc_test_3_install": [
          {
            "id": "avbf4tg7-evb3-df4t-avb4-vgr4gh6449ab",
            "stepId": "Vpc_test_3_install",
            "deploymentId": "8bg45f51-2bn4-fg33-vf5d-vbgf34fd79b6",
            "executionId": "7dvbfr44-9vb2-vfrd-vb43-gtr3fvgfa4b6",
            "nodeId": "Vpc_test_3",
            "instanceId": "0",
            "operationName": "delegate.install",
            "hasFailedTasks": false,
            "status": "COMPLETED"
          }
        ],
        ...
        "Port_install": [
          {
            "id": "cvgt4566-1vg4-4vbg1-cdff-2bnh56f4ga62",
            "stepId": "Port_install",
            "deploymentId": "8bg45f51-2bn4-fg33-vf5d-vbgf34fd79b6",
            "executionId": "7dvbfr44-9vb2-vfrd-vb43-gtr3fvgfa4b6",
            "nodeId": "Port",
            "instanceId": "0",
            "operationName": "delegate.install",
            "hasFailedTasks": true,
            "status": "COMPLETED"
          }
        ],
        ...
        "Private_install": [
          {
            "id": "evbger43-5vbg-df45-fg49-6vbg4rt53733",
            "stepId": "Private_install",
            "deploymentId": "8bg45f51-2bn4-fg33-vf5d-vbgf34fd79b6",
            "executionId": "7dvbfr44-9vb2-vfrd-vb43-gtr3fvgfa4b6",
            "nodeId": "Private",
            "instanceId": "0",
            "operationName": "delegate.install",
            "hasFailedTasks": false,
            "status": "COMPLETED"
          }
        ]
      },
      "stepTasks": {
        "Vpc_test_3_install": [
          {
            "id": "evbg3433-cvbg-gh41-vg33-1bhge4300d0a-0",
            "deploymentId": "8bg45f51-2bn4-fg33-vf5d-vbgf34fd79b6",
            "executionId": "7dvbfr44-9vb2-vfrd-vb43-gtr3fvgfa4b6",
            "workflowStepInstanceId": "avbf4tg7-evb3-df4t-avb4-vgr4gh6449ab",
            "operationName": "delegate.install",
            "nodeId": "Vpc_test_3",
            "instanceId": "0",
            "scheduleDate": 1602742452105,
            "status": "SUCCEEDED"
          }
        ],
        ...
        "Port_install": [
          {
            "id": "3bnhf33e-2vg2-vgf2-vgf4-5bgh34ff008b-0",
            "deploymentId": "8bg45f51-2bn4-fg33-vf5d-vbgf34fd79b6",
            "executionId": "7dvbfr44-9vb2-vfrd-vb43-gtr3fvgfa4b6",
            "workflowStepInstanceId": "cvgt4566-1vg4-4vbg1-cdff-2bnh56f4ga62",
            "operationName": "delegate.install",
            "nodeId": "Port",
            "instanceId": "0",
            "scheduleDate": 1602742456615,
            "status": "FAILED"
          }
        ],
        ...
        "Private_install": [
          {
            "id": "8bngf343-vb34-vb31-fg4b-vbgf45fg017b-0",
            "deploymentId": "8bg45f51-2bn4-fg33-vf5d-vbgf34fd79b6",
            "executionId": "7dvbfr44-9vb2-vfrd-vb43-gtr3fvgfa4b6",
            "workflowStepInstanceId": "evbger43-5vbg-df45-fg49-6vbg4rt53733",
            "operationName": "delegate.install",
            "nodeId": "Private",
            "instanceId": "0",
            "scheduleDate": 1602742461304,
            "status": "SUCCEEDED"
          }
        ]
      }
    },
    "error": null
  }

7. Retrieve the list of locations on which the current user can deploy the topology
===================================================================================

========  ================================================
Method    Path
========  ================================================
GET       /rest/latest/topologies/{topologyId}/locations
========  ================================================

Parameters
==========

Path Parameters

===============  ====================  ===============================
Paramters        Type                  Description
===============  ====================  ===============================
topologyId       string: <required>    Identifier of the topology
environmentId    string: <optional>    Identifier of the environment
===============  ====================  ===============================

Samples
-------

Sample Request

.. code-block:: bash

  curl \
      --header "Content-Type: application/json" \
      --header "Accept: application/json" \
       --header "X-Auth-Token: $OSTOKEN" \
      --request GET \
      https://designer.otc-service.com/rest/latest/topologies/domain_name_project_name_app_1:0.1.0-SNAPSHOT/locations?environmentId=72815d23-03df-473f-a647-c786989f7121

Sample Response

.. code-block::

  {
    "data": [
      {
        "location": {
          "groupPermissions": {
            "fg345g2e-5bg4-7868-9hj7-njh65g67e539": [
              "ADMIN"
            ]
          },
          "id": "3bnh56hf-6bg5-4hj6-bh6c-nhfb4564fd72",
          "name": "OTC",
          "orchestratorId": "45gh592c-bgh5-7823-bg54-bh456gh64g50",
          "infrastructureType": "OpenStack",
          "dependencies": [
            {
              "name": "yorc-types",
              "version": "1.1.0",
              "hash": "5687BBNG7FH3NFGH2JT58FHBF2C036560280108"
            },
            ...
            {
              "name": "otc-yorc",
              "version": "1.0.0",
              "hash": "NFH573HF7DH437FHD87W3HC8732HD892C7A993"
            }
          ],
          "metaProperties": {},
          "modifiers": [
            {
              "pluginId": "alien4cloud-yorc-provider",
              "beanName": "yorc-admin-network-modifier",
              "phase": "pre-node-match"
            },
            ...
            {
              "pluginId": "alien4cloud-yorc-provider",
              "beanName": "yorc-location-modifier",
              "phase": "post-matched-node-setup"
            }
          ],
          "creationDate": 1602830871186,
          "lastUpdateDate": 1602836718483
        },
        "orchestrator": {
          "id": "45gh592c-bgh5-7823-bg54-bh456gh64g50",
          "name": "Yorc",
          "pluginId": "alien4cloud-yorc-provider",
          "pluginBean": "yorc-orchestrator-factory",
          "deploymentNamePattern": "(application.id + '-' + environment.name).replaceAll('[^\\w\\-_]', '_')",
          "state": "CONNECTED"
        },
        "reasons": null,
        "ready": true
      }
    ],
    "error": null
  }