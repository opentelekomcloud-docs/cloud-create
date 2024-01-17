***********************
How to undeploy & purge
***********************

1. How to undeploy
==================

1. Go to the **Action** tab.
2. Click the **Undeploy** card and preview the uninstall workflow on the right panel.
3. Click **Launch Action** to start the uninstall workflow.

.. figure:: /_static/images/2-compute-undeploy.png
  :width: 800

  Figure 1. Undeploy an application

2. How to purge
===============

If the undeploy process encounters any errors, it stops with two options in the **Actions** tab:

1. **Resume**: To retry the failed step and continue the uninstall workflow. This option is useful when you want to retry the failed step due to a network connection error.
2. **Purge**: To skip the deletion of cloud sources and complete the uninstall workflow. However, you need to delete the cloud resources (via the console or APIs) **manually**.

.. figure:: /_static/images/2-compute-undeploy-resume.png
  :width: 800

  Figure 2. Resume and purge an undeployment