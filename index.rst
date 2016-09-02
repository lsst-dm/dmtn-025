:tocdepth: 1


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
and/or data sets they operate on scale up, it is desired to divide the workload
among all available, nowadays usually distributed, computing resources. It
invariably introduces additional levels of complexity related to load
balancing, data storage and transfer, monitoring tasks’ execution and handling
their failures.  Attempts to automate aforementioned aspects of the
orchestration process led in recent years to proliferation of frameworks and
libraries commonly known as **workflow management systems**. This `site`_ alone
lists over 70 existing, open-sourced WMS and is far from being complete.

At the time of writing this report (Aug, 2016), it is hard to point to any
clearly defined “winners” or, at least dominating, standards in the field.
Different projects focus on different aspects of an orchestration usually
dictated by direct needs of their authors and/or were designed to work in
certain “ecosystems”. Such issues cast some doubts on possibility of their
application in other domains or interoperability with technologies different
than intended. On top of that maturity of those projects, their support from
developers and community, available funding varies significantly, making a
selection of a WMS satisfying needs of the LSST project regarding **Batch
Production Services** far from being straightforward. In order to being able to
make, at least, educated guess which WMS would work best for the LSST project,
we decided to take a closer look on 8 the most “serious” looking WMS and survey
them all in more detail against a set of selected criteria we deem important
considering project’s needs.

Finally, it is worth emphasising that current survey has a preliminary
character. Its goal is *not* to select a future workflow management system for
the LSST project but to select 3 or 4 close contenders for further, more
elaborate, testing with LSST stack in HPC environment.

.. _site: https://github.com/common-workflow-language/common-workflow-language/wiki/Existing-Workflow-systems


Methodology of the survey
=========================

Our goal is to pick a WMS that will work in the batch system of LSST. To narrow
down the list of potential candidates to a smaller subset we have looked at
past and upcoming XSEDE conferences and workshops [XSEDE]_. Main reason to look
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

Before we compared the different workflow systems we created a set of criteria
that were used to evaluate the different systems. These criteria focused on
making sure that the workflow system that was picked is established and well
proven, since it should last for the project duration and we want to make sure
we can get help from the community both with questions, help with solving
problems and the ability to add back to the workflow system picked.


Flexibility in workflow generation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Workflow management systems use wide variety of methods to create workflows;
some uses dedicated GUIs, others follow “configuration as a code” approach and
a workflow description is a script written in Python.  Those methods offer
different degrees of freedom and flexibility for workflow’s authors and
therefore we consider this aspect an important characteristic of a workflow
management system.


Interoperability
^^^^^^^^^^^^^^^^

As we already mentioned in :ref:`intro` some of the existing workflow
management systems were designed to work in certain “ecosystems”. As the LSST
project’s execution environments may differ from the one a WMS was designed
for, it is essential to know what technologies a given system can cooperate
with currently or how easy it can be extended to support others.


Scalability
^^^^^^^^^^^

As workflows varies in size and complexity, it is important that a workflow
management systems is able to handle both small and large workflows (in terms
of number of tasks) equally well, e.g., that memory requirements grows
sufficiently slow with increasing number of tasks in a workflow.


Performance
^^^^^^^^^^^

Though scheduling tasks and tracking their provenance is not a particularly
CPU-intensive process, we wanted to be sure that under large load a workflow
management system will not become a bottleneck and will utilize available
resources efficiently.

           
Reliability
^^^^^^^^^^^

With large number of tasks running on distributed computational resources their
failures are inevitable. Thus any decent workflow management system should
support automatic **retries**, i.e., rerun the failing task, to be able to deal
with transient errors (e.g. a temporary network downtime). It should also track
tasks’ provenance to allow for a workflow **restart** in case of more permanent
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

*  communication channels allowing to either track current project’s
   development and report issues;
*  number of active developer and their responsiveness in addressing
   issues reported by users;
*  number of use cases, size of the project’s community and its activity
   on available project’s channels;
*  and last but not least, its funding.


Overall “user experience”
-------------------------

Finally, we decided to estimate the overall overhead associated with
getting started with a given workflow management system by installing
(almost) each of the surveyed framework on our workstations. That step
allowed us to address following questions:

*  **How thorough and complete is project’s documentation?** Is a user
   able to get all necessary information from it or has to use “Use the
   Force, Read the Source!” principle on regular basis?
*  **How complex is framework’s installation and configuration
   process?** Does it have relatively small number of dependencies or,
   on contrary, requires a multitude of external libraries to have all
   of its advertised features enabled?
*  **What is the learning curve associated with creating and executing
   an example workflow?**


Results
=======

We evaluated each project listed earlier based on criteria described in the
previous section using available online resources, i.e., white papers,
documentation, described use cases, and users’ opinions. Our findings best
summarizes the table below. ``N/A`` denotes pieces of information we were not
able to determine at the current level of scrutiny.

.. include:: include/table.txt

For sake of completeness, we also provide in-depth reviews concerning each
investigated workflow management system for an interested reader.


Summary
=======

LSST Batch Production Services [CONOPS]_ are about executing "[...] campaigns on
computing resources to produce the desired LSST data products [...]" where
**campaigns** are defined as sets of pipelines (ordered ensembles of
computational steps), inputs they are being run against, and methods handling
their outputs.  As campaigns can vary in size and complexity, we will face a
non-trivial problem of orchestration their execution on, possibly distributed,
computational resources to satisfy the data-dependencies. As this pattern is
quite common in many scientific and business applications there exist many
frameworks, workflow management systems, whose sheer purpose is to automate
such processes.

To select an optimal workflow management system for LSST Batch Services we made
a detailed, multi-aspect survey of a few available workflow management systems
to estimate their usefulness in implementing objectives of LSST batch
operations [CONOPS]_. Based on our findings, we have selected three of them:
Airflow, Pegasus, and PanDA for further tests in which picked candidates will
be used to orchestrate execution of a few specific LSST pipelines (yet to be
determined) in NCSA’s HPC environment.


.. include:: include/airflow.txt
.. include:: include/cloudslang.txt
.. include:: include/makeflow.txt
.. include:: include/panda.txt
.. include:: include/pegasus.txt
.. include:: include/pinball.txt
.. include:: include/radical.txt
.. include:: include/swift.txt


References 
==========

.. [CONOPS]
   Concept of Operations for the LSST Batch Production Services
   (`ref <http://ls.st/cr4>`).
.. [XSEDE]
   Workflow Systems on XSEDE, tutorial presented at XSEDE15 and XSEDE16
   (`ref <https://sites.google.com/site/xsedeworkflows/>`).


.. rubric:: Footnotes

.. [#] Staging data in and out must be included explicitly in the DAG
       representing a workflow with help of so called Transfer operators.
.. [#] Number of reported use cases is small but some of those projects are
       *huge*!
.. [#] Same as above.
.. [#] It is *not* compatible with `GNU GPL v3`_.
.. [#] Due to migration of services at UC Computation Institute, they are
   offline at the time we are writing this review.


.. _Apache License, v2.0: https://www.apache.org/licenses/LICENSE-2.0.html
.. _GNU GPL v2: https://www.gnu.org/licenses/old-licenses/gpl-2.0.html
.. _GNU GPL v3: https://www.gnu.org/licenses/gpl.html
.. _MIT License: https://opensource.org/licenses/MIT
.. _MySQL: https://www.mysql.com/
.. _PyPI: https://pypi.python.org/pypi
