============
Running Jobs
============

.. highlight:: console

Running L2 FP Through Populator Produced Scripts
================================================

The Populator supports creating scripts customized for several cluster management systems. The target cluster is selected using ``-t`` argument and one of the following strings:

========== ================
pbs_pro    PBS Professional
torque     Torque
pleiades   Pleiades Supercomputer
========== ================

The default cluster when the ``-t`` argument is not supplied is PBS Professional.

When the Populator is run with the ``-b`` option specifying a path to a L2 FP binary the following scripts may be created:

=============== =================
l2_fp_job.sh    Runs an individual sounding, created for all target clusters
launch_jobs.sh  Launches jobs on PBS Profession and Torque clusters
pleiades_job.sh Launches jobs on the Pleiades Supercomputer machines
=============== =================

You normally would not run ``l2_fp_job.sh`` directly, although it can be useful when debugging a single sounding. This script will take an argument specifying the index of which sounding to process. To run multiple instances of ``l2_fp_job.sh`` simultaneously on one computer you can use `GNU Parallels <http://www.gnu.org/software/parallel/>`_. For instance to run the first four soundings you would run it as follows::

    $ parallel ./l2_fp_job.sh ::: 0 1 2 3

The ``launch_jobs.sh`` script can only on be run from the head machine of a PBS Professional or Torque cluster, the machine where ``qsub`` exists. Any arguments specified to launch_jobs.sh will be passed directly to ``qsub``. On the SCF system the one required argument missing from the ``qsub`` call done in launch_jobs.sh is the argument specifying which queue to use. This argument is specified by the ``-q`` option and hence the normal use case for running launch_jobs.sh is as follows::

    $ launch_jobs.sh -q long

The above command will launch all soundings into the long queue. Should you wish to use a different queue just swap out "long" for the desired queue name. Any additional options for ``qsub`` can also be specified in the call to the launch_jobs.sh script.

Additional Cluster Documentation
================================

For full documentation on TORQUE job submission, please consult this document: `<http://www.clusterresources.com/torquedocs21/2.1jobsubmission.shtml>`_

For additional documentation on the TORQUE job submission command ``qsub``, please consult this document: `<http://www.clusterresources.com/torquedocs/commands/qsub.shtml>`_

PBS Professional documentation can be found here: `<http://www.pbsworks.com/SupportDocuments.aspx>`_

