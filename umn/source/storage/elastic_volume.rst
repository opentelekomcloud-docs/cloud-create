******************************************
How to attach and format an Elastic Volume
******************************************

.. _EVS:

1. How to attach a volume to a compute
======================================

* Under **Infrastructure**, drop the **BlockStorage** on the **Compute** component.

.. figure:: /_static/images/3-storage.png
  :width: 800

  Figure 1. Volume attachment

* Click on the **BlockStorage** and set the volume **size** (e.g., 10GB).

.. figure:: /_static/images/3-storage-properties.png
  :width: 800

  Figure 2. Set volume size

2. How to format and mount a volume at a given path
===================================================

* Drop the **LinuxFileSystem** on the Compute.
* Connect the **partition** point (on the right) of the LinuxFileSystem to the **feature** point (on the left) of the BlockStorage.

.. figure:: /_static/images/3-storage-format-disk.png
  :width: 800

  Figure 3. Format and mount a volume

* To configure the mount location and format type, click on the LinuxFileSystem:

  - **fs_type**: Choose a format type for the disk (e.g., :code:`ext4`).

  - **location**: Specify the mount point on the compute node (e.g., :code:`/mnt`).

.. seealso::
  The `LinuxFileSystem is a service catalog <https://github.com/opentelekomcloud-blueprints/alien4cloud-extended-types/blob/3.0.1/alien-extended-storage-types/types.yml>`_ executing on the given compute node. It mounts the BlockStorage to the compute node at a specified location and format it.
