====================
System Configuraiton
====================

.. highlight:: console

Memory Requirements
===================

Make sure your system has at least 2.5G of memory (including swap) available or else compilation may fail unexpectedly.

Python Requirements
===================

The Python bindings to the framework and support Python programs require the following Python packages. Most likely your Linux distribution already has packages available using it's respective package management tool. Otherwise, use the following links for details on installing each package:

* `Numpy/Scipy <http://www.scipy.org/scipylib/download.html>`_
* `Matplotlib <http://matplotlib.sourceforge.net/users/installing.html>`_
* `h5py <http://h5py.alfven.org/docs/guide/build.html>`_
* `PLY (Python Lex-Yacc) <http://www.dabeaz.com/ply/>`_
* `nosetests <http://readthedocs.org/docs/nose/en/latest/>`_

Optionally you may want to have the following Python packages installed to use some of the less commonly used utilities:

* `pyproj <http://code.google.com/p/pyproj/>`_
* `pyephem <http://pypi.python.org/pypi/pyephem/>`_

Compilers Versions
==================

One of the following combinations of compilers is needed:

* `GCC <https://gcc.gnu.org/>`_ and `GNU Fortran <http://gcc.gnu.org/fortran/>`_ at version 4.6 or greater
* GCC 4.6 or greater and `Intel Fortran Compiler <http://software.intel.com/en-us/intel-compilers/>`_  11.1 or newer

clang might work, but it has not been tested.

Linux System Setup
==================

Fedora/CentOS Deployment Steps
------------------------------

Under RHEL/CentOS you may need to update to a newer version of gcc/gfortran. If you are using RHEL/CentoOS 6 then you need to follow the `following instructions <https://gist.github.com/stephenturner/e3bc5cfacc2dc67eca8b>`_ to get gcc 4.8.

However if you are using RHEL/CentOS 7 or Fedora the default system packages have suitable versions of gcc::

    $ yum install gcc gcc-gfortran gcc-c++

A Fedora minimal system will also need the following::

    $ yum install make automake patch zlib-devel bzip2-devel

Install scripting languages needed by build process::

    $ yum install python ruby

Install ruby development packages needed for using gem to install ruby packages::

    $ yum install ruby-devel

For Fedora install the rubygems package::

    $ yum install rubygems

For RHEL/CentOS 6 install rubygems from source. Get the latest package from the `RubyGems.org <http://rubygems.org/pages/download>`_ website then install::

    $ tar zfvx rubygems-X.X.X.tgz
    $ cd rubygems-X.X.X
    $ ruby setup.rb

Note that the latest version of Ruby Gems might not work with CentOS's Ruby version. You may need to try earlier version.

For CentOS also install::

    $ yum install ruby-rdoc

Use rubygems to install narray::

    $ gem install narray

Follow :doc:`Setup <setup>` steps.

Compile according to :doc:`Compilation <compilation>` instructions for a Non JPL Build.

Ubuntu/Debian Deployment Steps
------------------------------

Install automake if you plan on developing the code and need to update the Makefile after adding files::

    $ apt-get install automake

Most other distributions provide gfortran along with gcc, but Ubuntu keeps it in separate packages. Install gfortran if not already installed::

    $ apt-get install gfortran

Install gdb if you would like to use that, it is not installed by default::

    $ apt-get install gdb

If you do not already have Subversion installed::

    $ apt-get install subversion

Install scripting languages and required supporting packages needed by build process::

    $ apt-get install python ruby rubygems libnarray-ruby

Install system library headers that are not installed by default::

    $ apt-get install libz-dev libbz2-dev

Install Python packages needed for support and operation tools::

    $ apt-get install python-ply python-h5py

Follow :doc:`Setup <setup>` steps.

Compile according to :doc:`Compilation <compilation>` instructions for a Non JPL Build.

OpenSuSE Deployment Steps
-------------------------

Install development packages using development patterns::

    $ zypper install -t pattern devel_C_C++ devel_basis devel_ruby

Install gfortran if not already installed::

    $ zypper install gcc-fortran

If you do not already have Subversion installed::

    $ zypper install subversion

Install Ruby support packages::

    $ gem install narray

Install system library headers that are not installed by default::

    $ zypper install zlib-devel libbz2-devel

Follow :doc:`Setup <setup>` steps.

Compile according to :doc:`Compilation <compilation>` instructions for a Non JPL Build.

OS X System Setup
=================

The following instructions are intended for those attempting to deploy on a Mac OS X system.

MacPort
-------

The standard GCC compiler is needed, using the Xcode compiler will not work as since they have switched to clang. Additionally, the software needs to be linked to the GNU version of glib. Although there are alternatives, we suggest you use MacPorts to obtain the necessary compiler tools. They have a page with more details on `selecting compiler versions <https://trac.macports.org/wiki/UsingTheRightCompiler>`_.

Install MacPort
^^^^^^^^^^^^^^^

See the `MacPorts install page <http://www.macports.org/install.php>`_ for directions on installing.

Install Packages
^^^^^^^^^^^^^^^^

Install packages using port::

    $ sudo port install glib2 gcc48 python34 hdf5 boost gsl

Select the installed packages to be used instead of the system packages::

    $ sudo port select --set gcc mp-gcc48
    $ sudo port select --set python python34

Follow :doc:`Setup <setup>` steps.

Compile according to :doc:`Compilation <compilation>` instructions for a Non JPL Build.
