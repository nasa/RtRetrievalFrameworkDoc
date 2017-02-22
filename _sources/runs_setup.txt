==========
Runs Setup
==========

.. highlight:: console

Populator Usage
===============

After following the details on the :doc:`Setup <setup>` page you should be ready to run the L2 FP support software needed to set up retrieval inputs.

Inside the ``level_2/support`` directory there is a tool known as "The Populator", which can generate the directory configuration needed by the L2 FP retrieval code. The setup scripts mentioned in previous sections will export this path to your environment so that you can run this program from anywhere on the system.

Populator Data Sources
======================

This tool can produce run directories for any of the following data sources:

* gosat - GOSAT flight data
* fts - TCCON FTS data
* oco - OCO downlooking data
* oco_uplooking - OCO uplooking data

At the minimum, for "gosat" or "oco" data sources the following files are needed:

* L1B HDF file (.hdf)
* Resampled ECMWF HDF file (.hdf)

The "fts" data source requires the following files:

* Run log file (.grl)
* A and B spectra files (.XXX where XXX is a 3 digit number)
* ASCII atmosphere file, which has been converted from GFIT's .mav file (.dat)

Populator Config File Creation
==============================

To use the Populator to set up a run directory, you will need to create a ``.config`` file that allows the Populator to find the various files it needs as well as to identify the data source.

At a minimum the config creation tool needs the data source type name and the path to the files mentioned in the last section for each data source type. In the following examples a bogus path is used for illustration purposes only. The path can be relative or absolute.

The Populator assumes that it will places all of its files inside of the directory where a configuration file is located. Therefore, it is best for each configuration to have its own directory.

Once you have created your directory chdir to it. Any of the follow examples are performed from the directory that serves as the base of your runs. The config file will live in the base directory.

For example, to create a config file for a "oco" data source you would run the following command from directory you created::

    $ create_config.py -t oco /path/to/oco_L1b_data.h5 /path/to/oco_Ecm_data.h5 

Once the command has finished executing you will have a new .config file with a name based upon the data source type and the current directory.

Optionally, you can specify a different config file name for output using the ``-o`` option. Additionally one can filter the sounding ids used based upon a user supplied list of sounding ids by providing the ``-l`` option.

For GOSAT data, the two polarization channels "S" and "P" are indicated by the appending of these letters to a sounding id. A pure numeral sounding id without either of these characters indicates that the S and P channels should be averaged.

To create a FTS config file you would use the "fts" data source and point to all the required files. For example::

    $ create_config.py -t fts /path/to/runlog.grl /path/to/spectra-a.001 /path/to/spectra-b.001 ... /path/to/atmosphere_from_gfit.dat

Note in the above the ellipsis indicate that you would specify ALL the spectra files as inputs on the command line. Using shell globbing is acceptable as all the filenames in a directory will be expanded before the shell passes control to the program. Additionally, ``create_config.py`` will ignore any files it does not know anything about.

Help text describing available options are displayed by running the program with the following option::

    $ create_config.py --help

Run Directory Population
========================

Once you have a config file you are ready to use the Populator program.

At a minimum this program needs the name of a config file and optionally the path to a L2 FP binary. If a path to a L2 FP binary is not specified then helper scripts for running the created inputs will not be created.

For example to populate the run directories for a config file created for an "oco" data source you would run the following command::

    $ populate.py -b /path/to/my/l2_fp oco_my_runs.config

Once the program has stopped executing you will have the following directories and files:

============================= =============================
sdos_input_list.dat           Contains versioning information as well as details on source files and directories used by the Populator
sounding_id.list              List of sounding ids to be processed.
l2_fp_job.sh, launch_jobs.sh  When the "-b" option is given to populate.py this file these files are created to aid in running the software as explained in a later section.
============================= =============================

When ``l2_fp_job.sh`` is run for the first time, the following directories will be created:

============================= =============================
log                           Directory where log files for per sounding runs are written
output                        Directory where HDF5 output files are written
============================= =============================
