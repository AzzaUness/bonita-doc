
1.12.2 Environments
===================

[](){#top}
An environment is a set
of configuration definitions for a project. You can configure a process for
several environments. In this way, one process can run in several environments
without any modification, because the information that is specific to an
environment is set at configuration. In the Performance, Efficiency, and Teamwork editions, Bonita BPM Studio proposes three
environments, Local,  Production, and Qualification. You can also define
custom environments.

You can configure, run, and debug a
process in a specific environment.

The default environment is the one
most recently selected in the Configure, Run, or Debug menus in the Cool
bar.

[Defining a
custom environment](#define_custom)\
[Configuring a process in an environment](#configure_environment)\
[Running a
process in an environment](#run_environment)\
[Debugging a process in an environment](#debug_environment)

[](){#define_custom}Defining a custom
environment
-------------------------------------

To define a custom environment for a process, open
the process in Bonita BPM Studio Performance, Efficiency, or Teamwork edition and
select the pool. Then choose **New environment...** from
the **Configure**, **Run**, or
**Debug** menu in the Cool bar, enter a name for the new
environment, and click ***OK***. The new environment will
appear in the  **Configure**, **Run**, and
**Debug** menus in the Cool bar.

[](){#configure_environment}Configuring a process
in an environment
-------------------------------------------------

To configure a process for an environment, select the
environment from the **Configure** menu in the Cool bar. Then use
the configuration wizard to [configure the process](/configuring-process-bonita-bpm-studio-0).

[](){#run_environment}Running a process in an
environment
---------------------------------------------

To run a process from Bonita BPM Studio in an
environment, select the environment from the **Run** menu in the
Cool bar. The process will be launched using the information configured for the specified
environment.

[](){#debug_environment}Debugging a process in
an environment
----------------------------------------------

Use Debug in Bonita BPM Studio to disconnect any connectors
that you know cannot run successfully when you run the process in development or
test mode.To debug a process from Bonita BPM Studio in an environment, select the
environment from the **Debug** menu in the Cool bar. Specify the
connectors to be skipped, then click ***Debug***. The
process will be launched using the information configured for the specified
environment but the specified connectors will
be ignored.

