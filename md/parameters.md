
1.6.4 Parameters
================

You can use a parameter to store a value that differs between deployments but is fixed for all cases of a deployed process.
The value is set when you configure the process. For example,
if you have a process for approving expense reports, you could define a parameter for the approver’s email address that is set at deployment.
If you are using the Performance or Efficiency edition, the value can also be changed after deployment.

[Defining a process parameter](#defining_a_process_parameter)\
[Configuring parameter values](#configuring_parameter_values)\
[Parameter usage in forms](#parameter_usage_in_forms)

Defining a process parameter {#defining_a_process_parameter}
----------------------------

You can define a parameter for a pool or for a lane. A parameter can be of type text, Boolean, integer, or double.

To define a parameter:

1.  Open the process diagram and select the pool or lane.
2.  In the Data tab, go to the Parameters pane. A table of the parameters already define is displayed.
3.  Click **Add**, enter the parameter name, choose the type from the drop-down list, and optionally add a description of the parameter.

To modify a parameter (for example to change its type), click on the existing type on in the table of parameters and choose the new type from the drop-down list.

To remove a parameter, select it in the table of parameters and click **Remove**.

Configuring parameter values {#configuring_parameter_values}
----------------------------

Parameter values are set in Bonita BPM Studio when the process is configured (see the [process configuration overview](/process-configuration-overview-0) for other details of how to configure a process).

To set a parameter value:

1.  Open the process diagram in Bonita BPM Studio.
2.  Choose an environment from Configure menu in the cool bar.
3.  Click **Parameters** in the left-hand menu of the configuration wizard. A table of process parameters and their current values is displayed.
4.  Select the table cell for the parameter value you want to set or modify, and enter the new value.
5.  When all the parameters values have been set, click **Finish**.

You can export the parameter values to a file by clicking **Export parameters as file...**. The default file name is <span class="tt">*process\_name*\_Parameters.properties</span>.
You can also set parameter values by importing a <span class="tt">parameter.properties</span> file. The values in the file are used to set the values of the corresponding parameters.
Note that importing a <span class="tt">parameters.properties</span> file does not create parameters in the process, just sets the values of those that exist if they are present in the file.

When you build a process for deployment (see [Import and export a process](/import-and-export-process-1)), the <span class="tt">.bar</span> archive that is
created also contains a <span class="tt">parameters.properties</span> file.

You can edit a <span class="tt">parameters.properties</span> file to change the value of a parameter before importing it into Bonita BPM Studio or before deploying the <span class="tt">.bar</span>.
You can also edit the file after process deployment, but this is not recommended if the process is running. If you need to
change a parameter value for a process that is in production, [update the process configuration in Bonita BPM Portal](/processes-0#modpar)).

Parameter usage in forms {#parameter_usage_in_forms}
------------------------

From 7.x, to use parameters in forms, you need to retrieve them using the [REST API.](/bpm-api-1#processparameter)

