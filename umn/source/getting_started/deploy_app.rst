****************************************
How to choose a cloud provider to deploy
****************************************

1. Go to: **Deploy**
2. Choose an environment to deploy (e.g., **DEVELOPMENT**).

.. figure:: /_static/images/2-compute-choose-env.png
  :width: 800

  Figure 1. Choose an environment to deploy

.. tip::
  You can create multiple environents (e.g., development, test, production). In each enviroment, you can deploy an application in a cloud provider. To create an additional environment, click on **Configure Environments**.

3. Choose **OTC**. This will deploy the application in the region :code:`eu-de` of Open Telekom Cloud.
4. (Optional) Click on **Configure cloud provider** if you wish to configure any properties of Open Telekom Cloud resources (e.g., keypair or flavor of a Compute etc.)
5. Click **Deploy** to start.

.. figure:: /_static/images/2-compute-deploy-otc.png
  :width: 800

  Figure 2. Deploy the application on OTC

.. note::
  Cloud Create deploys your application on Open Telekom Cloud for free of charge (i.e., you only need to pay for the cloud resources you create on Open Telekom Cloud).
  To deploy your application on Google Cloud, refer to the :ref:`Google deploy`.