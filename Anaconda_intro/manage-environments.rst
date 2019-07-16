================
What is Anaconda
================
Anaconda is a free and open-source distribution of the Python and R programming languages for scientific computing, that aims to simplify package management and deployment
=============
What is conda
=============
 Package versions are managed by the package management system called conda

=======================
How to Install Anaconda
=======================
1. In your browser, download the Anaconda installer for Linux, link : https://www.anaconda.com/download/#linux
2. Installation
 .. code::
 
    bash ~/Downloads/Anaconda3-2019.03-Linux-x86_64.sh

=====================

=====================
Managing environments
=====================

.. contents::
   :local:
   :depth: 1

With conda, you can create, export, list, remove, and update
environments that have different versions of Python and/or
packages installed in them. Switching or moving between
environments is called activating the environment. You can also
share an environment file.


.. note::
   ``conda activate`` and ``conda deactivate`` only work on conda 4.6 and later versions.
   For conda versions prior to 4.6, run:

      * Windows: ``activate`` or ``deactivate``
      * Linux and macOS: ``source activate`` or ``source deactivate``

Creating an environment with commands
=====================================

.. tip::
   By default, environments are installed into the ``envs``
   directory in your conda directory. Run ``conda create --help``
   for information on specifying a different path.

Use the terminal or an Anaconda Prompt for the following steps:

#. To create an environment:

   .. code::

      conda create --name myenv

   .. note::
      Replace ``myenv`` with the environment name.

#. When conda asks you to proceed, type ``y``:

   .. code::

      proceed ([y]/n)?

  This creates the myenv environment in ``/envs/``. This
  environment uses the same version of Python that you are
  currently using because you did not specify a version.

3. To create an environment with a specific version of Python:

   .. code-block:: bash

      conda create -n myenv python=3.4

4. To create an environment with a specific package:

   .. code-block:: bash

      conda create -n myenv scipy

   OR:

   .. code-block:: bash

      conda create -n myenv python
      conda install -n myenv scipy

5. To create an environment with a specific version of a package:

   .. code-block:: bash

      conda create -n myenv scipy=0.15.0

   OR:

   .. code-block:: bash

      conda create -n myenv python
      conda install -n myenv scipy=0.15.0

6. To create an environment with a specific version of Python and
   multiple packages:

  .. code-block:: bash

     conda create -n myenv python=3.4 scipy=0.15.0 astroid babel

  .. tip::
     Install all the programs that you want in this environment
     at the same time. Installing 1 program at a time can lead to
     dependency conflicts.

To automatically install pip or another program every time a new
environment is created, add the default programs to the
:ref:`create_default_packages <config-add-default-pkgs>` section
of your ``.condarc`` configuration file. The default packages are
installed every time you create a new environment. If you do not
want the default packages installed in a particular environment,
use the ``--no-default-packages`` flag:

.. code-block:: bash

  conda create --no-default-packages -n myenv python

.. tip::
   You can add much more to the ``conda create`` command.
   For details, run ``conda create --help``.


.. _create-env-from-file:

Creating an environment from an environment.yml file
====================================================

Use the terminal or an Anaconda Prompt for the following steps:

#. Create the environment from the ``environment.yml`` file:

   .. code::

      conda env create -f environment.yml

   The first line of the ``yml`` file sets the new environment's
   name. For details see :ref:`Creating an environment file manually
   <create-env-file-manually>`.


#. Activate the new environment: ``conda activate myenv``

   .. note::
      Replace ``myenv`` with the environment name.


#. Verify that the new environment was installed correctly:

   .. code::

      conda list


Cloning an environment
=======================

Use the terminal or an Anaconda Prompt for the following steps:

You can make an exact copy of an environment by creating a clone
of it:

.. code::

   conda create --name myclone --clone myenv

.. note::
   Replace ``myclone`` with the name of the new environment.
   Replace ``myenv`` with the name of the existing environment that
   you want to copy.

To verify that the copy was made:

.. code::

   conda info --envs

In the environments list that displays, you should see both the
source environment and the new copy.


.. _activate-env:

Activating an environment
=========================

Activating environments is essential to making the software in the environments
work well. Activation entails two primary functions: adding entries to PATH for
the environment, and running any activation scripts that the environment may
contain. These activation scripts are how packages can set arbitrary
environment variables that may be necessary for their operation.

To activate an environment: ``conda activate myenv``

.. note::
   Replace ``myenv`` with the environment name or directory path.

Conda prepends the path name ``myenv`` onto your system command.

Windows is extremely sensitive to proper activation. This is because
the Windows library loader does not support the concept of libraries
and executables that know where to search for their dependencies
(RPATH). Instead, Windows relies on a standard library search order, defined at
https://docs.microsoft.com/en-us/previous-versions/7d83bc18(v=vs.140). If
environments are not active, libraries won't get found and there will be lots
of errors. HTTP or SSL errors are common errors when the
Python in a child environment can't find the necessary OpenSSL library.

Conda itself includes some special workarounds to add its necessary PATH
entries. This makes it so that it can be called without activation or
with any child environment active. In general, calling any executable in
an environment without first activating that environment will likely not work.
For the ability to run executables in activated environments, you may be
interested in the ``conda run`` command.


Deactivating an environment
===========================

To deactivate an environment, type: ``conda deactivate``

Conda removes the path name for the currently active environment from
your system command.

.. note::
   To simply return to the base environment, it's better to call ``conda
   activate`` with no environment specified, rather than to try to deactivate. If
   you run ``conda deactivate`` from your base environment, you may lose the
   ability to run conda at all. Don't worry, that's local to this shell - you can
   start a new one.


.. _determine-current-env:

Determining your current environment
====================================

Use the terminal or an Anaconda Prompt for the following steps.

By default, the active environment---the one you are currently
using---is shown in parentheses () or brackets [] at the
beginning of your command prompt::

  (myenv) $

If you do not see this, run:

.. code::

   conda info --envs

In the environments list that displays, your current environment
is highlighted with an asterisk (*).

By default, the command prompt is set to show the name of the
active environment. To disable this option::

  conda config --set changeps1 false

To re-enable this option::

  conda config --set changeps1 true


Viewing a list of your environments
===================================

To see a list of all of your environments, in your terminal window or an
Anaconda Prompt, run:

.. code::

   conda info --envs

OR

.. code::

   conda env list

A list similar to the following is displayed:

.. code::

   conda environments:
   myenv                 /home/username/miniconda/envs/myenv
   snowflakes            /home/username/miniconda/envs/snowflakes
   bunnies               /home/username/miniconda/envs/bunnies


Viewing a list of the packages in an environment
================================================

To see a list of all packages installed in a specific environment:

* If the environment is not activated, in your terminal window or an
  Anaconda Prompt, run:

  .. code-block:: bash

     conda list -n myenv

* If the environment is activated, in your terminal window or an
  Anaconda Prompt, run:

  .. code-block:: bash

     conda list

To see if a specific package is installed in an environment, in your
terminal window or an Anaconda Prompt, run:

.. code-block:: bash

   conda list -n myenv scipy


.. _pip-in-env:

Using pip in an environment
===========================

To use pip in your environment, in your terminal window or an
Anaconda Prompt, run:

.. code-block:: bash

   conda install -n myenv pip
   conda activate myenv
   pip <pip_subcommand>


Sharing an environment
=======================

You may want to share your environment with someone else---for
example, so they can re-create a test that you have done. To
allow them to quickly reproduce your environment, with all of its
packages and versions, give them a copy of your
``environment.yml file``.

Exporting the environment file
-------------------------------

.. note::
   If you already have an ``environment.yml`` file in your
   current directory, it will be overwritten during this task.

#. Activate the environment to export: ``conda activate myenv``
   
   .. note::
      Replace ``myenv`` with the name of the environment.

#. Export your active environment to a new file::

     conda env export > environment.yml

   .. note::
      This file handles both the environment's pip packages
      and conda packages.

#. Email or copy the exported ``environment.yml`` file to the
   other person.

Removing an environment
=======================

To remove an environment, in your terminal window or an
Anaconda Prompt, run:

.. code::

   conda remove --name myenv --all

You may instead use ``conda env remove --name myenv``.

To verify that the environment was removed, in your terminal window or an
Anaconda Prompt, run:

.. code::

   conda info --envs

The environments list that displays should not show the removed
environment.
