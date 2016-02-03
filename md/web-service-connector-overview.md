
1.8.19 Web service connector overview
=====================================

<div id="top">

</div>

<div id="call">

</div>

Generic configuration
---------------------

1.  Go to Web services in the connectors list.
2.  Select **Web Service Client.**
3.  Click on ***Next***
4.  Enter a name and description for the connector.
5.  Click on ***Next.***

To format your entries according to SOAP specifications, go to [SOAP!](http://www.w3.org/TR/soap12-part1/#intro)

See a working example of a web service connector in this [tutorial](/web-service-connector-tutorial-0).

<div class="alert alert-warning">

<span class="glyphicon glyphicon-warning-sign"></span> Warning: this connector uses the StAX API. StAX libraries cannot be loaded in more than one classloader but must instead be placed in a single location where they can be called by all the items that use them.
When you configure a process that uses this connector, you must [manage the jar files](/manage-jar-files-0).

</div>

Input
-----

Input type
Description
Type
Target NS
Target NS (URL)
String
Service name
Name of web service
String
Port name
Web service port name
String
Request
Request entity
String
End point address
URL
String
Binding
Specify protocol and data format
String
Password
Password of user authorized to access
String
Service NS
Target namespace (URL)
String
Service name
Name of web service
String
Port name
Web service port name
String
End point address
URL
String
Binding
Specify protocol and data format
String
Output
------

| Output type | Description         | Type   |
|-------------|---------------------|--------|
| Response    | Result of execution | Source |

Result
------

The connector will locate and execute the specific web service and return and output in the form of a Java object.

