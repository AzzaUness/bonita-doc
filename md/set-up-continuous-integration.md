
4.12.3 Set up continuous integration
====================================

This page explains how to set up a continuous integration environment for your Bonita BPM processes.

With Continuous Integration (CI) your processes are continuously built and tested while you are designing them. Collaborating on process design can be enhanced with CI by
ensuring the integrity of your processes along the development phase.

[Prerequisites](#prereq)\
[Set up a Bonita BPM Studio environment for CI](#env)\
[Create a Jenkins job to build your processes automatically](#jenkins)\
[Test your processes automatically](#test)

You could use any integration server, but this page describes how to use Jenkins, as an example.

Prerequisites {#prereq}
-------------

To follow this page, you need to be familiar with:

-   Java-based project development with Maven
-   Subversion version control system use
-   Jenkins CI server installation and configuration

Before you begin, you need the following components available for your CI environment:

-   Bonita BPM Studio Teamwork, Efficiency, or Performance edition:
    The automated process build feature is not available in the Community edition.
-   A window manager:
    You need a window manager to use the Bonita BPM Studio process builder.
-   Java:
    You need Java 1.6 to be installed on the CI server and your development computer.
-   Maven:
    This guide provides example Maven-based projects for executing automated tests of your processes.
    Maven 3 is required on the CI server and the test development computer.
-   Subversion:
    Bonita BPM Studio enables business analysts and developers to collaborate on process design using a shared Subversion (SVN) repository.
    This tutorial assumes that you have access to a Subversion server.
-   Jenkins:
    The [Jenkins CI](http://jenkins-ci.org/) server is responsible for coordinating continuous build and test of your processes.
    This tutorial assumes that Jenkins is up and running with Maven and Subversion plugins.

Set up Bonita BPM Studio for CI {#env}
-------------------------------

There are two stages to setting up Bonita BPM Studio for CI:

1.  On your Subversion server, create a shared repository that can be accessed by the business analysts and developers working on processes and by the CI tools.
    After you have create your shared repository you may develop your processes in this repository and/or import existing processes. Each Bonita BPM Studio user must connect to the repository.
2.  Define an environment for configuring processes for CI. This environment will be stored in the shared repository together with your process definitions.
    The next section assumes that you created an environment called CI.

Create a Jenkins job to build your processes automatically {#jenkins}
----------------------------------------------------------

This section describes how to create a Jenkins job to build your processes automatically.
This job will produce Business Archive (.bar) files ready for deployment.

All example scripts given on this page are compatible with Unix-like operating systems.

1.  Prepare Bonita BPM Studio on the CI server

    Bonita BPM Studio includes a BonitaStudioBuilder script for building processes in a CI environment. Install Bonita BPM Studio as follows:

    a.  Download the OS-independent package (zip) from the customer portal.
        For example use BonitaBPMSubscription-6.1.0.zip for version 6.1.0. You must have the same version of Bonita BPM Studio for the shared repository and the CI server.
    b.  Extract the package to a permanent location on the CI server:
        ` $> unzip -d /path/to/BonitaBPMStudio BonitaBPMSubscription-6.1.0.zip`
    c.  Install your license (a license must have been requested for CI server):
        ` $> cp license.lic /path/to/BonitaBPMStudio/BonitaBPMSubscription-6.1.0/lic_folder/`

    You are recommended to install a window manager on the CI server in order to have process diagram screenshots generated along with business archives.

2.  Create Jenkins job

    In Jenkins, create a new job of type “Build a free-style software project”.
    Specify a job name for example “BonitaBPM-BuildProcesses”.

3.  Configure Jenkins job

    a.  Check out your process repository from Subversion. To do this, configure the “Source Code Management” section to retrieve (check out) your Subversion process shared repository.
        Specify the repository URL, and optionally your local repository. We recommend that you set teh check-out strategy to *Use 'svn update' as much as possible*.
    b.  Invoke BonitaStudioBuilder script

        Note: only one instance of BonitaStudioBuilder can be executed at a time on a computer.

        Add a build step of type “Execute shell” configured with the following content:

        ` #!/usr/bin/env sh  echo "##### Clean previously generated business archives" rm -Rf $WORKSPACE/process-bars echo ""  echo "##### Clean BonitaStudioBuilder workspace before each execution" rm -Rf /path/to/BonitaBPMStudio/workspace echo ""  echo "##### Execute BonitaStudioBuilder for all processes" cd /path/to/BonitaBPMStudio/.. ./BonitaBPMStudio/workspace_api_scripts/BonitaStudioBuilder.sh -repoPath=$WORKSPACE/process-repository -outputFolder=$WORKSPACE/process-bars -buildAll -environment=CI  echo "##### Package generated business archives" zip $WORKSPACE/process-bars.zip $WORKSPACE/process-bars/*`{type="xml"}

        This sample script invokes BonitaStudioBuilder to build all processes from your process-repository using the CI environment.

        See the [BonitaStudioBuilder documentation](/automating-process-builds-1) for information on script options.

    c.  Share generated business archives as a Maven artifacts

        One way to make the generated .bar files available to other Maven projects is to install them as Maven artifacts.
        The following example installs the processes artifact in the local Maven repository.

        The Maven artifact could also be deployed to a centralized Maven repository manager solution.
        To do so, add a second build step of type “Invoke top-level Maven targets” configured as follows:

        ` Goals: install:install-file  Properties: groupId=com.acme.bonita artifactId=process-bars version=1.0.0-SNAPSHOT packaging=zip file=$WORKSPACE/process-bars.zip`{type="xml"}

    d.  Archive generated artifact in Jenkins

        You can archive the job artifact (generated processes package) in Jenkins.
        To do so, add a post-build action of type “Archive the artifacts” and choose to archive the “process-bars.zip” package.
        As a result, the generated business archives will be made available for download from Jenkins interface.

Run the Jenkins job

Run the “BonitaBPM-BuildProcesses” Jenkins job. When it is finished, the Maven artifact `com.acme.bonita:process-bars:1.0.0-SNAPSHOT` in installed in the local Maven repository of the CI server.
The generated processes package is also available as a job build artifact in Jenkins.

Test your processes automatically {#test}
---------------------------------

This section contains an example of how to test a process from a given Business Archive. It consists of writing JUnit Test cases using the Bonita BPM Engine Java API.

Note: In this example, we show only how to test the runtime aspects of a process, using the Java APIs.
It is also possible to use cargo to deploy the generated bar file into an application server and then launch Selenium tests to test web aspects of a process.

For this example we are using a Maven project to write our tests.

1.  In your IDE create a new Maven project and share it (for example using SVN or Git).
2.  [Configure local access](/configuring-bonita-home-client-1) to Bonita BPM Engine.
3.  As we want to test processes build with a Bonita BPM Subscription edition, you need to [configure the required Maven artifacts](/create-your-first-project-engine-apis-and-maven-0#maven_dependencies).
    You should also check that there is a valid license file in `${bonita.home.folder}/server/licenses`.
4.  We recommend that you write your test cases in the src/test/java folder of your project and put all related resources (Bar files, organization file…) in src/test/resources.
5.  Before installing your processes load the relevant organization (regarding your actor mapping). You may have to export your organization from a Bonita BPM Studio:
    Menu Organization &gt; Export, Select your Organization.

    For example:

    ` private void installOrganization(){ File organizationFile = new          File(MyTestCase.class.getResource("/ACME.xml").getFile()); String organizationContent = getFileContent(organizationFile); getIdentityAPI().importOrganization(organizationContent); }`{type="java"}

6.  Then as a basic test, we try to deploy each generated processes. For example:
    ` @Test public void deploy() throws Exception{ //Retrieve automatically generated bars as a Map Map bars = getBars();     Assert.assertTrue("No bar found in resources",!bars.isEmpty());  //For each bar deploy and enable it for(Entry entry : bars.entrySet()){     BusinessArchive archive = BusinessArchiveFactory.readBusinessArchive(entry.getValue()) ;         final String entryKey = entry.getKey();     ProcessDefinition def = getProcessAPI().deploy(archive);         final long defId = def.getId();         Assert.assertNotNull("Failed to deploy "+entryKey,def);         getProcessAPI().enableProcess(defId);         getProcessAPI().disableProcess(defId);         getProcessAPI().deleteProcessDefinition(defId); } }`{type="java"}

Now configure a job to run this simple test case on your CI
(these steps assume you have shared your generated processes as a Maven artifact, so you can use the maven-dependency plugin to retrieve the latest built processes):

1.  Create a new freestyle job in Jenkins
2.  Configure the source code management to retrieve your Maven project.
3.  Add a build step
4.  Select “Invoke top-level Maven targets”
5.  Use following Goal: org.apache.maven.plugins:maven-dependency-plugin:2.7:get
6.  In properties, set the following:
    -   version=1.0.0-SNAPSHOT
    -   dest=\$WORKSPACE
    -   groupId=com.acme.bonita
    -   artifactId=process-bars
    -   packaging=zip

<div style="text-align: center;">

![Get the processes](images/images-6_0/Get_processes.png "Get the processes")
<div class="caption" style="clear: both">

<span style="display: block;color: #BC071B;text-align: center;margin-top: 10px">Get the processes</span>

</div>

</div>

1.  Then add another build step to unzip the artifact. Select “Execute shell”, and use
    the command `unzip process-bars-1.0.0-SNAPSHOT.zip -d project/src/test/resources`.

<div style="text-align: center;">

![Unzip the processes](images/images-6_0/Unzip_processes.png "Unzip the processes")
<div class="caption" style="clear: both">

<span style="display: block;color: #BC071B;text-align: center;margin-top: 10px">Unzip the processes</span>

</div>

</div>

1.  Finally, add another Maven 3 build step to build the test project:
    -   Goals: clean install

<div style="text-align: center;">

![Build the test project](images/images-6_0/Invoke_Maven_Test_Project.png "Build the test project")
<div class="caption" style="clear: both">

<span style="display: block;color: #BC071B;text-align: center;margin-top: 10px">Build the test project</span>

</div>

</div>

You may want to publish the JUnit report:

-   Add a post build action -&gt; Publish Junit test result.
-   Set the path `project/target/test-reports/*.xml`.

<div style="text-align: center;">

![Publish a JUnit report](images/images-6_0/Post_Build_Actions.png "Publish a JUnit report")
<div class="caption" style="clear: both">

<span style="display: block;color: #BC071B;text-align: center;margin-top: 10px">Publish a JUnit report</span>

</div>

</div>

