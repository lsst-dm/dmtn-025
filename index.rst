:tocdepth: 2

.. _scope:

Scope of document
=================

This document summarizes the results of the survey regarding viable candidates
for a workflow management system (WMS) to be used by LSST Batch Production
Services [CONOPS]_. A WMS helps automate orchestration and execution of large
batches of tasks on available, possibly distributed, computational resources.

.. _intro:

Introduction
============
            
A common pattern in scientific computing involves the execution of many
computational or data manipulation tasks. Those tasks are usually coupled,
i.e., data produced by of one task are consumed by one or more other tasks.
Thus execution of such tasks often requires a non-trivial coordination
(orchestration) to satisfy their data dependencies. As the numbers of tasks
and/or data sets they operate on scale up, it is desirable to divide the workload
among all available, nowadays usually distributed, computing resources. This
invariably introduces additional levels of complexity related to load
balancing, data storage and transfer, monitoring tasks’ execution and handling
their failures.  Attempts to automate aforementioned aspects of the
orchestration process have led in recent years to the proliferation of frameworks and
libraries commonly known as **workflow management systems**. This `site
<http://ls.st/5pc>`__ alone lists over 70 existing, open-sourced WMS and is far
from being complete.

At the time of writing this report (Aug 2016), it is hard to point to any
clearly defined “winners” or, at least dominating, standards in the field.
Different projects focus on different aspects of an orchestration which are usually
dictated by direct needs of their authors and/or were designed to work in
certain “ecosystems”. Such issues cast some doubts on the possibility of their
application in other domains or interoperability with technologies different
than intended. On top of that, the maturity of those projects, their support from
developers and community, and available funding varies significantly, making a
selection of a WMS satisfying needs of the LSST project regarding **Batch
Production Services** far from being straightforward. In order to being able to
make, at least, educated guess which WMS would work best for the LSST project,
we decided to take a closer look at 8 of the most “serious”-looking WMS's and survey
them all in more detail against a set of selected criteria we deem important
considering project’s needs.

Finally, it is worth emphasising that this current survey has a preliminary
character. Its goal is *not* to select a future workflow management system for
the LSST project but to select 3 or 4 close contenders for further, more
elaborate, testing with the LSST stack in HPC environment.

.. _methodology:

Methodology of the survey
=========================

Our goal is to pick a WMS that will work in the batch system of LSST. To narrow
down the list of potential candidates to a smaller subset we have looked at
past and upcoming XSEDE conferences and workshops [XSEDE]_.  The main reason to look
at the XSEDE workshops is that all of the workflows discussed at these
workshops are capable of running on the XSEDE resources and thus are ready for
large scale executions (Makeflow, Pegasus, RADICAL-Pilot, and Swift). Besides
the workflows discussed there we considered some of the more recent workflows
systems that have been open sourced by different large companies (Airflow,
CloudSlang, and Pinball). Finally we looked at other fields such as high energy
physics and picked one of the prominent workflows from there (Panda). Each of
the candidates was then reviewed against the set of criteria which may be
divided into three main categories described in more detail below.

Technical merits
----------------

Before we compared the different workflow systems we created a set of
criteria that were used to evaluate the different systems. These
criteria focused on making sure that the workflow system that was
picked is established and well-proven, since it should last for the
project duration, and we want to make sure we can get help from the
community both with questions, help with solving problems and the
ability to add back to the workflow system chosen.

Flexibility in workflow generation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Workflow management systems use wide variety of methods to create workflows;
some uses dedicated GUIs, others follow “configuration as a code” approach and
the workflow description is a script written in Python.  Those methods offer
different degrees of freedom and flexibility for the authors of the workflow and
therefore we consider this aspect an important characteristic of a workflow
management system.

Interoperability
^^^^^^^^^^^^^^^^

As we already mentioned in the :ref:`intro` some of the existing workflow
management systems were designed to work in certain “ecosystems”. As the LSST
project’s execution environments may differ from the one a WMS was designed
for, it is essential to know what technologies a given system can cooperate
with currently or how easy it can be extended to support others.

Scalability
^^^^^^^^^^^

As workflows varies in size and complexity, it is important that a workflow
management system is able to handle both small and large workflows (in terms
of number of tasks) equally well, e.g., that memory requirements grows
sufficiently slow with increasing number of tasks in a workflow.

Performance
^^^^^^^^^^^

Though scheduling tasks and tracking their provenance is not a particularly
CPU-intensive process, we wanted to be sure that under a large load a workflow
management system will not become a bottleneck and will utilize available
resources efficiently.
           
Reliability
^^^^^^^^^^^

With a large number of tasks running on distributed computational
resources some failures are inevitable. Thus any decent workflow
management system should support automatic **retries**, i.e., rerun
the failing task, to be able to deal with transient errors (e.g. a
temporary network downtime). It should also track the provenance of
tasks to allow for a workflow **restart** in case of more permanent
execution errors (e.g.  runtime errors due to flawed input).
                
Monitoring tools
^^^^^^^^^^^^^^^^

Workflow management systems provide logs which can be used to monitor tasks
execution. However, with numerous large and complex workflows, going through
logs is not a particularly useful method to monitor workflow execution in a
real time. That is why in our survey we decided to take into account existence
of any extra monitoring tools such as dashboards.

Available support
-----------------

Considering the scale, complexity, and longevity of the LSST project, we are
convinced that purely technical merits cannot be the only ones to be taken into
account during the evaluation process. After all, every piece of software has
bugs which need to be fixed, configuration issues which may not be covered in
documentation, and so on. Thus, we also decided to consider things like:

*  communication channels allowing tracking of a current project’s
   development and reporting of issues;
*  the number of active developer and their responsiveness in addressing
   issues reported by users;
*  the number of use cases, size of the project’s community and its activity
   on available project’s channels;
*  and last but not least, its funding.

Overall “user experience”
-------------------------

Finally, we decided to estimate the overall overhead associated with
getting started with a given workflow management system by installing
(almost) each of the surveyed frameworks on our workstations. That step
allowed us to address following questions:

*  **How thorough and complete is project’s documentation?** Is a user
   able to get all necessary information from it or does he or she have to use the “Use the
   Force, Read the Source!” principle on regular basis?
*  **How complex is framework’s installation and configuration
   process?** Does it have a relatively small number of dependencies or,
   on contrary, does it require a multitude of external libraries to have all
   of its advertised features enabled?
*  **What is the learning curve associated with creating and executing
   an example workflow?**

.. _results:

Results
=======

We evaluated each project listed earlier based on criteria described
in the previous section using available online resources, i.e., white
papers, documentation, described use cases, and users’ opinions. Our
findings is best summarized in :numref:`survey-summary` (entries
marked with a question mark denote pieces of information we were not
able to determine at the current level of scrutiny).  For sake of
completeness, we also provide farther below more in-depth reviews
concerning each investigated workflow management system for an
interested reader.

.. include:: include/table.txt

.. _summary:

Summary
=======

LSST Batch Production Services [CONOPS]_ are about executing "[...] campaigns on
computing resources to produce the desired LSST data products [...]" where
**campaigns** are defined as sets of pipelines (ordered ensembles of
computational steps), inputs they are being run against, and methods of handling
their outputs.  As campaigns can vary in size and complexity, we will face a
non-trivial problem of orchestrating their execution on potentially distributed
computational resources while satisfying the data-dependencies. As this pattern is
quite common in many scientific and business applications, there exist many
frameworks called "workflow management systems", whose sole purpose is to automate
such processes.

To select an optimal workflow management system for LSST Batch Services we made
a detailed, multi-aspect survey of a few available workflow management systems
to estimate their usefulness in implementing objectives of LSST batch
operations [CONOPS]_. Based on our findings, we have selected three of them:
Airflow, Pegasus, and PanDA for further tests in which the chosen candidates will
be used to orchestrate execution of a few specific LSST pipelines (yet to be
determined) in NCSA’s HPC environment.

.. _followon:

Follow-on to the Summary for Status Oct 2019
============================================

Following this workflow management system survey, the first BPS prototype workflows were
implemented in Pegasus for runtime execution.  These workflows were leveraged against
some small test datasets (e.g., testdata_ci_hsc package).
These were executed on the 'Verification Cluster' of servers run by NCSA for LSST DM by first submitting
HTCondor glide-in jobs to the Slurm scheduler ('pilot jobs' that effectively transform the Slurm compute
nodes into a Condor pool.) The BPS prototype workflows were
also the basis for small scale tests in the LSST DM AWS POC work.

Within its workflow execution Pegasus utilizes HTCondor DAGMan; in some sense DAGMan is the
inner engine driving the work of the Directed Acyclic Graphs, with Pegasus serving as a
layer of management/organization,  monitoring, and additional features/plugins, etc., 
wrapping HTCondor DAGMan and the Condor layer at which jobs run.

Following these small scale tests, the BPS workflows are now targeting support for large scale production
and operations.  The BPS effort is taking this opportunity to undergo a recasting, with the plan
to construct and wrap HTCondor DAGMan workflows directly. These DAGs will submit and manage
HTCondor jobs to pools of resources.  This offers a simplification to the development, reducing the number
of layers/tools as the BPS is integrated with the Science Dags, Quantum Graphs, etc., of the Gen3 system.
The DM team will also utlize enhanced direct collaboration with the HTCondor team (e.g., in
efforts such as the DM AWS POC) to assist with the integration with HTCondor DAGMan workflows.
The HTCondor DAGMan based BPS workflows will be tested on the nascent and increasing pool
of HTCondor batch system nodes at NCSA.

In addition to DM work of prototyping BPS workflows against HTCondor clusters running at NCSA, the
DM AWS POC effort is investigating the execution of workflows on HTCondor pools constructed via
the condor_annex mechanism within AWS cloud. Testing has demonstrated that full HTCondor pools, central manager
and worker nodes, can be launched and prototype BPS workflows executed for the first case of testdata_ci_hsc.
The condor_annex approach continues to evolve and improve in its usability.
In the past (at the HTCondor 8.6 level) it was advisable to utilize base AMIs
that the HTCondor team provided / made publically available. In current work at the HTCondor 8.9.3 level, we see
that worker nodes AMIs conformable to the condor_annex can be readily created by installing a few condor packages,
and that customized configuration can be deployed to worker nodes at the time of condor_annex invocation.
The DM AWS POC effort will continue tests at increasingly larger scales.
In local testing we can demonstrate the annex of AWS worker nodes to an on-premises central manager at
NCSA, though full security and trust between such AWS workers and LSST DM HTCondor resources at NCSA
requires further investigation.

.. include:: include/addendum.txt
.. include:: include/airflow.txt
.. include:: include/cloudslang.txt
.. include:: include/makeflow.txt
.. include:: include/panda.txt
.. include:: include/pegasus.txt
.. include:: include/pinball.txt
.. include:: include/radical.txt
.. include:: include/swift.txt

.. _references:

References 
==========

.. [CONOPS]
   Concept of Operations for the LSST Batch Production Services
   (http://ls.st/cr4).
.. [XSEDE]
   Workflow Systems on XSEDE, tutorial presented at XSEDE15 and XSEDE16
   (https://sites.google.com/site/xsedeworkflows/).


.. _Apache License, v2.0: https://www.apache.org/licenses/LICENSE-2.0.html
.. _GNU GPL v2: https://www.gnu.org/licenses/old-licenses/gpl-2.0.html
.. _GNU GPL v3: https://www.gnu.org/licenses/gpl.html
.. _MIT License: https://opensource.org/licenses/MIT
.. _MySQL: https://www.mysql.com/
.. _PyPI: https://pypi.python.org/pypi
