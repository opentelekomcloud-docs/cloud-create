***********************************
How to save your application in git
***********************************

When you design your application, you can push changes to and pull from a remote Git repository:

1. How to push to a remote Git repository
=========================================

* Create a new Github repository (e.g., :code:`test-git`).

.. figure:: /_static/images/git-create-repo.png
  :width: 800

  Figure 1. Create a new git repo

* In the design editor, click **Remote** to config the git url (e.g., :code:`https://github.com/trihoangvo/test-git.git`).

.. figure:: /_static/images/git-remote-config.png
  :width: 800

  Figure 2. Config git url

* Design your application and **save** your work.
* Click **Push**:
    * If using **Github**, specify the **access token** in the **password** field.
    * If using **Gitlab**, specify the your **password** in the **password** field.

.. figure:: /_static/images/git-push.png
  :width: 800

  Figure 3. Push to git

Expected result
---------------

Your design is pushed to git in the given branch (e.g., :code:`main`):

.. figure:: /_static/images/git-push-2.png
  :width: 800

  Figure 4. Result of git push

.. note::

    * We never save your git password so you have to input it everytime.
    * If there is a conflict, git will push to a new branch starting with :code:`conflicts-*`

2. How to pull changes to your application
==========================================

* Create a new file (e.g., :code:`my_script.sh`) in your **local** git:

.. figure:: /_static/images/git-pull.png
  :width: 800

  Figure 5. Git create new file

* Push the new file to your **remote** git:

.. code-block::

    git add my_script.sh
    git commit -m "Add my script"
    git push origin main

* Click **Pull** to update your application:

.. figure:: /_static/images/git-pull-2.png
  :width: 800

  Figure 6. Pull changes from git

Expected result
---------------

The new file :code:`my_script.sh` shows up in the editor under **Active content**:

.. figure:: /_static/images/git-pull-3.png
  :width: 800

  Figure 7. Result of git pull

.. note:: **Repository size limit**: The pulled remote repository is not allowed to be bigger than 20 Mb.

