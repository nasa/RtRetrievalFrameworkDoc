=======
Testing
=======

.. highlight:: console

Unit Testing
============

There are two kinds of tests available. The first is unit testing. These test individual pieces of the system. The focus of this testing is on the software itself: did it build correctly, are the individual classes calculating what we expect them to.

The unit tests are fully automated. You can run them with the following command from a build directory::

    $ make check

This runs through all the unit tests that don't take a long time to run, and in the end says if the tests are successful or not (i.e., there is no analysis on your part, the test either succeeds or it doesn't).

When running the unit tests, it can be useful to work in a debug build. This means the configuration used the ``--enable-debug`` flag. This runs slower, but it adds additional checks such as range checking in both the C+ and Fortran arrays.

The target ``fast_check`` works just like ``check`` except it does not build any of the executables. You will not catch any problems introduced into the build of the top level executables. This test target is faster and used in instances where one is developing a class and only running unit tests. The target ``check`` should still be run after the major work on a class has been completed.

A longer, more complete set of tests can be run using the command::

    $ make long_check

This runs all the tests that ``make check`` does, plus additional tests that take longer to run. For an optimized build (without the ``--enable-debug`` flag in the configuration), this takes a few minutes to run. A debug version takes longer to run, but still runs in under an hour.

Just like with ``make check``, the long checks print out if the tests are successful or not.

Support exists for specifying specific unit tests to run. For instance so you can do::

    $ make fast_check run_test=lidort_driver/*

The command above will run just the lsi_rt unit tests. This feature passes the string through to the ``--run_test`` argument of the Boost unit tests, so consult the `Boost documentation <http://www.boost.org/doc/libs/1_55_0/libs/test/doc/html/utf/user-guide/runtime-config/run-by-name.html>`_ to see what can be specified.

This method does not catch breakage to any other classes, running the full suite should still be done occasionally to make sure everything is working.

Unit Testing Variables
======================

The ABSCO tables are needed for certain unit tests. Once you have a copy of the ABSCO table files you can specify the location of these files when doing the initial configuration by specifying ``--with-absco=<dir>``. You can also override the default on the make command line used for testing, for example::

    $ make fast_check run_test=lidort_driver/* abscodir=/path/to/my/copy/absco

End-to-End Test
===============

A build target ``run_tests`` exists which runs l2_fp for several tests (unless it hasn't changed since the last run), and then compares the results against expected results, printing out the differences.  Individual tests are run by specifying the build name with the suffix ``_run``. The tests names can be seen by looking at the subdirectories under the ``tests/`` directory of the source code.

For example, to run all tests, in your build directory run::

    $ make run_tests

To test only a OCO-2 test case use the following target::

    $ make oco2_sounding_1_run

Or, to test only a GOSAT case use the following::

    $ make tccon_sounding_1_run
