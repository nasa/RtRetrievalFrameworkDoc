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

Fortran Compilers
=================

One of the following Fortran compilers is required for compiling:

* `GNU Fortran <http://gcc.gnu.org/fortran/>`_ -- 4.5 or greater
* `Intel Fortran Compiler <http://software.intel.com/en-us/intel-compilers/>`_  -- 11.1 or newer

Linux System Setup
==================

Gentoo Deployment Steps
-----------------------

Install gdb if you would like to use that, it is not installed by default::

    $ emerge -av gdb

If you do not already have Subversion installed::

    $ emerge -av subversion

Install scripting languages and required supporting packages needed by the build process::

    $ emerge -av ruby rubygems

The Ruby narray package is also required, but may only be available in the Gentoo testing branch. If you have not enabled testing for your architecture you would have to do the follow (if your architecture is amd64)::

    $ echo 'ACCEPT_KEYWORDS="~amd64"' > /etc/make.conf
    $ emerge -av narray

We also require the installation of Python but by default Gentoo satisfies that through the eselect-python package. Note that emerging the dev-lang/python will install Python 3.x which is not yet used by our software.

Install Python packages for support utils::

    $ emerge -av numpy
    $ emerge -av h5py
    $ emerge -av ply

Follow :doc:`Setup <setup>` steps.

Compile according to :doc:`Compilation <compilation>` instructions for a Non JPL Build.

Fedora/CentOS Deployment Steps
------------------------------

Under CentOS you may need to update to a newer version of gcc/gfortran. CentoOS is conservative with GCC updates and keep newer versions in separate packages::

    $ yum install gcc44 gcc44-gfortran gcc44-c++

Fedora has the latest GCC in the default packages::

    $ yum install gcc gcc-gfortran gcc-c++

A Fedora minimal system will also need the following::

    $ yum install make automake patch zlib-devel bzip2-devel

If you do not already have Subversion installed::

    $ yum install subversion

Install scripting languages needed by build process::

    $ yum install python ruby

Install ruby development packages needed for using gem to install ruby packages::

    $ yum install ruby-devel

For Fedora install the rubygems package::

    $ yum install rubygems

For CentOS (also possibly Redhat) install rubygems from source. Get the latest package from the `RubyGems.org <http://rubygems.org/pages/download>`_ website then install::

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

Select MacPort or Fink
----------------------

Additional tools are needed on the Mac to build. There are two different projects that supply these tools, Macport and Fink. The two projects are similar, it is a matter of preference which one you select. If you don't otherwise care, then use MacPort, it is what was used during development.

MacPort
-------

If you select MacPort, then follow the directions in this section.

Install MacPort
^^^^^^^^^^^^^^^

Look at `<http://www.macports.org/install.php>`_ for directions on installing macport.

Install Packages
^^^^^^^^^^^^^^^^

Install packages using port::

    $ sudo port install gcc44 python27 py27-ipython hdf5-18 boost gsl

Configuration
^^^^^^^^^^^^^

The configuration is slightly tricky, if you want to have python working. The issue is that you need to use the same version of gcc and g+ used to create python, while at the same time using a different version of gfortran. The configuration used in development is::

    $ ../Level2/configure \
       FCLIBS="/opt/local/lib/gcc44/gcc/x86_64-apple-darwin10/4.4.5/libgfortranbegin.a /opt/local/lib/gcc44/libgfortran.dylib" \
       FC=/opt/local/bin/gfortran-mp-4.4 F77=/opt/local/bin/gfortran-mp-4.4 \
       --with-python-swig --with-lidort=build --with-cppad=build --with-blitz=build

Note the "FCLIBS" - this is required. This works around splitting between different versions for g+ and gfortran. Without this, the python library will use the libstdc+ coming from g+-4.4, not the system one. This results in fairly obscure memory errors when std::string from one version of g is passed to a library expecting a different version.

Note that the strict requirement on the compiler is only needed for building the python wrappers. If you don't care about the wrappers, you can leave off the FCLIBS and use the same version for gfortran and for g+ and gcc. For nonpython development, the configuration we used is::

    $ ../Level2/configure THIRDPARTY=build FC=/opt/local/bin/gfortran-mp-4.4 CXX=/opt/local/bin/g++-mp-4.4 \
        CC=/opt/local/bin/gcc-mp-4.4 F77=/opt/local/bin/gfortran-mp-4.4 --enable-debug --enable-maintainer-mode

In this case, we build all the thirdparty libraries rather than using the system versions, this is needed so that a consistent compiler is used for things link HDF5 and BOOST.

Fink
----

If you select Fink, then follow the directions in this section.

Upgrade Xcode
^^^^^^^^^^^^^

Upgrade to Xcode 3.1.x for 10.5, 3.2.x for 10.6 from Apple Developers Connection. Go to the `Developer Connection <http://connect.apple.com/>`_ member site then select downloads to find the desired Xcode version.

Xcode needs to be upgraded so that Fink dependencies can be meet otherwise an error like this will show up:
"xcode (>= 3.1.2)" for package "gcc44-4.4.1-1000"

Install Fink
^^^^^^^^^^^^

`Download Fink <http://www.finkproject.org/download/>`_ and follow instructions: `<http://www.finkproject.org/download/index.php?phpLang=en>`_

Enable Fink Unstable
^^^^^^^^^^^^^^^^^^^^

Enable fink unstable packages according to these instructions: `<http://www.finkproject.org/faq/usage-fink.php?phpLang=en#unstable>`_

Install Fink Packages
^^^^^^^^^^^^^^^^^^^^^

Install free Fortran compilers::

    $ fink install g77 g95

Install Python packages::

    $ fink install python26 numpy-py27 h5py-py27 matplotlib-py27 scipy-py27 ipython-py27

Since we are using the Fink unstable repository everything comes from source and the compilation which includes many dependencies can takes a fair amount of time.

As a consequence of the dependencies of the above packages the latest GCC is installed and hence gfortran will also be available.
