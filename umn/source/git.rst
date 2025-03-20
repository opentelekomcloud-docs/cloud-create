***********************************
How to save your application in git
***********************************

When you design your application, you can push changes to and pull from a remote Git repository:

1. How to push to a remote Git repository
=========================================

1.1. Create a new Github repository
-----------------------------------

* Go to Github and create a new repository (e.g., :code:`test-git`):

.. figure:: /_static/images/git-create-repo.png
  :width: 700

  Figure 1. Create a new git repo

.. important::
  Leave the options :code:`Add a README file` and :code:`Add .gitignore` unchecked. Otherwise, you will have a conflict error when Cloud Create pushes to the repository the first time.

1.2. Create a Github access token (if you don't have one)
---------------------------------------------------------

1. Go to **Settings** / **Developer Settings** / **Personal Access Tokens** / **Tokens (classic)**
2. Choose :code:`Write:packages` and :code:`Delete:packages`.
3. Backup the token.

.. figure:: /_static/images/git-create-access-token.png
  :width: 800

  Figure 2. Create a personal access token with write and delete permissions

1.3. Design & push
------------------

* Design and **Save Application**.
* In the design editor, click **Git configuration**

.. figure:: /_static/images/git-config.png
  :width: 800

  Figure 3. Config git url

* And set the git url (e.g., :code:`https://github.com/trihoangvo/test-git.git`).

.. figure:: /_static/images/git-remote-config.png

  Figure 4. Set git url

* Choose **Push**:
    * Type the branch name (e.g., **main** for Github, **master** for Gitlab).
    * Specify the Github **access token** / Gitlab **password** in the **password** field.

.. figure:: /_static/images/git-push.png
  :width: 800

  Figure 5. Push to git

.. note::

  * We never save your access token or password so you have to input it everytime.

Expected result
---------------

Your design is pushed to git in the given branch (e.g., :code:`main`):

.. figure:: /_static/images/git-push-2.png
  :width: 800

  Figure 6. Result of git push

.. note::

    * If there is a conflict, git will push to a new branch starting with :code:`conflicts-*`

2. How to pull changes to your application
==========================================

* Clone your git repository (e.g., :code:`git clone git@github.com:trihoangvo/test-git.git`)
* Create a new file (e.g., :code:`my_script.sh`) in your **local** git:

.. figure:: /_static/images/git-pull.png
  :width: 800

  Figure 7. Git create new file

* Push the new file to your **remote** git:

.. code-block::

    git add my_script.sh
    git commit -m "Add my script"
    git push origin main

* Click **Pull** to update your application:

.. figure:: /_static/images/git-pull-2.png
  :width: 800

  Figure 8. Pull changes from git

Expected result
---------------

The new file :code:`my_script.sh` shows up in the editor under **Active content**:

.. figure:: /_static/images/git-pull-3.png
  :width: 800

  Figure 9. Result of git pull

.. note:: **Repository size limit**: The pulled remote repository is not allowed to be bigger than 20 Mb.

