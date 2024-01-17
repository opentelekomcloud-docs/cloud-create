*********************************
How to upload a file to a compute
*********************************

Step 1. Upload a local file
===========================

You can use the Bash component to upload any files to a compute.

1. Go to **Artifacts** / **Upload** artifact.
2. Select **Local file**.
3. Upload a file and select it (e.g., :code:`readme.txt`).

.. figure:: /_static/images/7-Bash-write-script-upload-local.png
  :width: 800

  Figure 1. Upload a local file

(Alternative) Download a remote file
====================================

Alternatively, you can specify a remote URL to download a file from github or gitlab to the compute.

1. Go to  **Artifacts** / **Upload** artifact.
2. Select **Remote file**.
3. Provide the URL of the remote file.

.. figure:: /_static/images/7-Bash-write-script-upload-remote.png
  :width: 800

  Figure 2. Upload a remote file

For example:

* URL: :code:`https://raw.githubusercontent.com/opentelekomcloud-blueprints/tosca-tutorials/master`
* File: :code:`README.md`

In this example, the file `README.md from Github <https://raw.githubusercontent.com/opentelekomcloud-blueprints/tosca-tutorials/master/README.md>`_ will be downloaded to the compute node.

.. note:: We allow to download files from the following domains: .gitlab.com, .github.com, .githubusercontent.com, .t-systems.com, .rubygems.org, .treasuredata.com, .letsencrypt.org, .galaxy.ansible.com, .googleapis.com.



Step 2. How to use the uploaded file
====================================

* Go to **Artifacts** / **Configure**.
* Provide a script (e.g., :code:`bash_1_configure.sh`)

.. figure:: /_static/images/7-Bash-write-script-upload-configure.png
  :width: 800

  Figure 3. Write a script to use the uploaded file

* with the following codes:

.. code-block:: bash

  #!/bin/bash

  echo "Path to upload file: $upload"

  # print the content of the file.
  cat $upload

In the script, the environment variable :code:`$upload` holds the path to the file on the compute.

Expected result
===============

When the Bash component is executed on the compute, the deployment logs will print out the path to the uploaded file (e.g., :code:`/home/ubuntu/.yorc.../readme.txt`) and its content.

.. figure:: /_static/images/7-Bash-write-script-upload-result.png
  :width: 800

  Figure 4. Deployment log shows path to the uploaded file