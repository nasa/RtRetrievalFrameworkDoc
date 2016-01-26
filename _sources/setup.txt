=====
Setup
=====

.. highlight:: console

The following sections describe how to obtain and initialize the tools and source code needed to develop and utilize the resources available for running the RT Retrieval Framework code. These sections describe a recommended directory structure that can be changed by the user. Operators/developers should feel free to reorganize where they deploy the tools. The organization has been designed to not necessitate any one strict directory organization.

Obtaining the Code
==================

The `public copy <https://github.com/nasa/RtRetrievalFramework>`_ of the source code can be obtained from Github. 

:: 

    $ git clone https://github.com/nasa/RtRetrievalFramework.git 

This is a clone of the development repository used at JPL. On a regular basis we push our changes to the public repository from JPL.

Enviroment Setup
================

The downloaded directory contains a bash script that will include into your shell environment paths and variables needed by various tools contained therein. These scripts should be sourced in your bash startup script in order for these tools to be configured and ready to use each time you log in.

Add the following to your .bashrc or wherever you place these things for yourself::

    $ source /path/to/source/setup_env.sh

This script is used when working on code developing. On the :doc:`compilation` page it will be discussed how to use the software as an installed program.

Development Environment Organization
====================================

The development environment checked out in the preceding section has the following important sub-directories:

=================  ========================
lib/               Where most source code lives
input/             Input files and Lua configuration
support/           Supporting utilities
tests/             End-to-End tests
unit_test_data/    Inputs and expected results used by unit tests
=================  ========================
