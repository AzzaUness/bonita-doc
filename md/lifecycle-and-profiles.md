
1.2.5 Lifecycle and profiles
============================

[](){#top}
This page explains the lifecycle of creating an application or a process using Bonita BPM and putting it into production. At different stages in the
lifecycle, different people with different knowledge and skills play
roles. These people are identified by profiles.

<div style="text-align: center;">

![Lifecycle and profiles diagram](images/images-6_0/lifecycle-app.png "Lifecycle and profiles")
<div class="caption" style="clear: both">

<span style="display: block;color: #BC071B;text-align: center;margin-top: 10px">Lifecycle and profiles</span>

</div>

</div>

There are three major phases in the lifecycle: [application creation](#process_creation),
[system creation](#system_creation), and [production](#production). This page introduces these three phases, and explains
how the Bonita BPM components use [profiles](#components) to manage the features presented to users.

Application creation {#process_creation}
--------------------

During this phase, the business analyst specifies the processes that are in the application and
[creates the process diagrams](/diagram-overview-0). The application developer takes the
specification of each process and creates a running process, adding forms, scripts
or Java code if required. Each process is tested and debugged. It
can also be evaluated using simulations. When a process is
complete, it is exported ready for deployment.

This phase needs only the Bonita BPM Studio component of Bonita BPM.
See the [Bonita BPM Studio installation overview](/bonita-bpm-studio-installation-2) for details of how to install Bonita BPM Studio for process
creation.

The application developer also creates the application pages. When the pages are complete, they are exported and deployed in Bonita BPM Portal. The application developer then builds the application, specifying the menus and navigation.

System creation {#system_creation}
---------------

During this phase, which typically runs in parallel with the end
of the application creation phase, the platform administrator prepares
the infrastructure needed to support the deployed application. This
infrastructure includes Bonita BPM, and prerequisites such as a
database and an application server. There are many options for how
Bonita BPM can be installed: see the [Installation overview](/bonita-bpm-installation-overview-0) for details.

Preparing the system also includes [defining the organization](/organization-overview-0), with all the users, groups, and roles
required for running processes and applications. Bonita BPM Studio contains an example organization, ACME, which you can use for
testing.

When configuration is complete, the system is ready for application
deployment. The administrator deploys the processes and the application pages, and updates the configuration as needed, including [mapping process
actors](/actors-1#Mapping_an_actor) to users in the organization.

Production
----------

To [deploy a process](/processes-0#installanapp), you import it into Bonita BPM Portal and start it. After the process is started, process instances can be started. The details
of how process instances are started depend on the process
definition.

To deploy an application, you import pages into Bonita BPM Portal and then use the application builder to specify the application menus and navigation.

After deployment, you can use the Bonita BPM Portal [live update](/live-update) options to update an application or process that is in production.

Profiles and Bonita BPM components {#components}
----------------------------------

Bonita BPM Studio and Bonita BPM Portal manage the features available
according to the profile of the user.

In Bonita BPM Studio, a user selects a profile at login, and can
change profile at any time. The application developer
profile gives access to all features of Bonita BPM Studio. You can
also configure the features that are visible to your profile,
hiding items that are not relevant to your activity, to make the
interface cleaner and easier to navigate.

In Bonita BPM Portal, a profile is associated to a login, and by
default only a user with the administrator profile can
[change the profile of a user.](/profiles-overview-2) A user can have any number of profiles. It is also possible to define [custom profiles](/custom-profiles-2).

