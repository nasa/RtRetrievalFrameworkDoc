===========
Compilation
===========

.. highlight:: console

The RT Retrieval Framework uses the standard autotools used by many open source software. While you can build the software in the same directory that you have the source, it is suggested that you use a separate build directory. This allows you to do different builds from the same source, e.g. an optimized and debug version.

Building
========

There is some third party software included with the source code that need compiling along with our code. To build the framework's code along with the third party software, execute the following::

    $ mkdir build_optimized
    $ cd build_optimized
    $ /your/path/to/rtr_framework/configure THIRDPARTY=build
    $ make all

In the above replace ``/your/path/to/rtr_framework`` with the location where you downloaded the software during the :doc:`setup` instructions.

The ``configure`` command creates the Makefile to use. You only need to run this when you are creating a new build directory, after that you can just rerun ``make all`` to get any software updates. Configure will check a bunch of things on the system, and in the end print a report something like::

     Level 2 Full Physics is now configured

      Installation directory:       /your/path/to/build_optimized/install
      Build debug version:          no
      Fortran compiler type:        ifort
      Fortran compiler:             /opt/local/depot/intel/11.1/064/bin/intel64/ifort -g -xSSE2 -O3 -Difort -heap-arrays 1024
      C compiler:                   gcc -g -O2
      LD Flags:                      -R /opt/local/depot/intel/11.1/064/lib/intel64:/opt/local/depot/intel/11.1/064/mkl/lib/em64t:/opt/local/depot/intel/11.1/064/lib/intel64:/opt/local/depot/intel/11.1/064/mkl/lib/em64t

      HDF5 support:                 yes
      Build own HDF5 library:       yes
      Build own LIDORT library:     yes
      Install documentation:        no

If you are building from within your source checkout you can just use the following script included with the source::

    $ ./nonjpl_build.script

In order to run all of the unit tests, you'll also need to have a copy of the ABSCO (Absoprtion Coefficient) tables. These files must be obtained separately from JPL.

For serious development it is recommended to compile the third party software once and then refer to it in subsequent builds::

    $ mkdir build_third_party
    $ cd build_third_party
    $ /your/path/to/rtr_framework/configure THIRDPARTY=build --prefix=/install/path/for/third_party
    $ make thirdparty

Now subsequent builds can use this directory instead of recompiling the third party packages::

    $ /your/path/to/rtr_framework/configure THIRDPARTY=/install/path/for/third_party
    $ make all

Build Results
=============

The executable is ``l2_fp``, and will be placed in the top of the build directory.

Another useful target is ``install``, which will build ``l2_fp`` and copy it and all associated libraries to a separate install directory. The default directory used is ``./install`` unless you specify an alternative to ``configure``. This is primarily of use for production to put the executable files in a separate location. For a developer, you generally don't need to do this step.

Using Other Compilers
=====================

The configure script will search for one of the supported compilers -- ifort, f90, f95, gfortran, g95, pathf90, pgf90 - in that order.

Note that we only test with the Intel compiler "ifort" and "gfortran". We require a fairly new version of ifort (>= 11.1) and gfortran (>= 4.5).

If you want to use a different compiler than what configure automatically selects, you can specify it on the configure line as ``FC=<compiler>``, e.g., ``FC=f90``.

The thirdparty version of HDF-5 can only be built using ifort or gfortran. Other compilers will likely build if you don't try to build hdf5 (i.e., you don't have ``--with-hdf5=build``).

Build Options
=============

The third party libraries can be selectively compiled by specifying configure options such as ``--with-hdf5=build``, or ``--with-lidort=build``. This will build your own version of these libraries. This is particularly useful if you are actually modifying, for instance the LIDORT code (e.g., a bug fix), but still want to refer to a prebuilt directory of third party software. The full list of third party libraries can be seen by running configure with the ``--help`` option.

There are a few other options that can be passed to configure:

======================== ===========
Option                   Description 
======================== ===========
--help                   Print out all the options for configure 
--enable-debug           Build a version of l2_fp for debugging, rather than an optimized version
--enable-maintainer-mode Automatically update configure, Makefile.in. Useful if you are editing the various ".am" files 
--with-absco=<dir>       Specify location of ABSCO data used by unit tests 
--prefix=<dir>           Specify directory to install to 
FC=<compiler>            Specify a compiler to use 
THIRDPARTY=build         Build a local copy of all the third party libraries 
THIRDPARTY=<dir>         Search in <dir> in addition to the normal locations for third party libraries 
======================== ===========

Debugging
=========

It has been discovered that gfortran works better with valgrind and debugging (although our "official" builds use ifort). Therefore if you need to use valgrind or gdb and have ifort installed in your path, you will need to directly specify the Fortran compiler as gfortran when creating debug builds::

    $ /path/to/level_2/configure FC=gfortran --enable-debug <other config options>

Developer Information
=====================

For developers who need to understand autotools in more detail, an nice introduction is found at `Autotools: a practitioner's guide to Autoconf, Automake and Libtool <http://www.freesoftwaremagazine.com/books/autotools_a_guide_to_autoconf_automake_libtool>`_, and detailed reference can be found at `Autobook <http://sources.redhat.com/autobook/autobook/autobook_toc.html>`_.
