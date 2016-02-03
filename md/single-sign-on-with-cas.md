
2.4.3 Single sign-on with CAS
=============================

This pages explains how to configure your Bonita BPM Platform system to use CAS to provide single sign-on (SSO). It assumes you already have a working CAS service. All Bonita BPM users must be registered in CAS.

This information applies to a Bonita BPM platform deployed from a bundle, not to the Engine launched from Bonita BPM Studio.

[Configure Bonita BPM Engine and JBoss for CAS](#conf_engine_jboss)\
[Configure Bonita BPM Engine and Tomcat for CAS](#conf_engine_tomcat)\
[Configure Bonita client for CAS](#conf_client)\
[Configure logout behaviour](#logout_option)\
[Manage passwords](#passwords)\
[LDAP synchronizer and CAS](#ldap)\
[Single sign-on with CAS using the REST API](#rest)

CAS configuration is at tenant level. Each tenant can use a different CAS service.

Note: On a system using CAS to manage logins, if a user who is not already logged in tries to access a page in the Portal by clicking on a URL link, they are re-directed to the login page.
After logging in, the requested page is not displayed automatically. The user must click the link again.

Configure Bonita BPM Engine and JBoss for CAS {#conf_engine_jboss}
---------------------------------------------

The deploy bundle contains the files needed to use CAS with Bonita BPM platform and a JBoss 7 application server. They are contained in `cas-module.zip`.
You can use this zip file to configure CAS for a platform deployed from the JBoss bundle or from the deploy bundle. The `cas-module.zip` archive contains some jar files that are required.
It also contains a configuration file for the module, `module.xml`, which defines the jar files to be loaded from the module itself and the dependencies of the module. For example:

``{type="xml"}
For a standard installation, it is not usually necessary to modify this file.

To configure Bonita BPM Engine for CAS:

1.  If you do not already have it, download the Subscription edition deploy zip from the customer portal.
2.  Add the CAS module. To do this, copy `cas-module.zip` to `JBOSS_HOME/modules` and unzip it to merge the CAS module with the existing modules.
3.  Make the CAS module global so that it can be used by any application. To do this, edit `JBOSS_HOME/standalone/configuration/standalone.xml` and change the definition of the `ee` subsystem to the following:
4.  Edit `JBOSS_HOME/standalone/configuration/standalone.xml` and add the BonitaAuthentication module.
    before the `` tag, insert these lines (specifying the relevant IP addresses and port numbers):
    ``{type="xml"}

    Note: The security-domain name must be unique, BonitaAuthentication-1 in the above example.

5.  In the `CasLoginModule` configuration, check that the `principalGroupName` property is set to `CallerPrincipal`. This is required to retrieve the username from the Bonita application.
    Bonita BPM uses the CAS LoginModule in the JASIG implementation, so see the CAS LoginModule section of the [Jasig documentation](https://wiki.jasig.org/display/CASC/JAAS+Integration) for more information.
6.  Update `bonita-tenant-sp-custom.properties`. If you edit this in `bonita-home/engine-server/conf/tenants/template/`, the settings will apply to all new tenants.
    For an existing tenant, edit the properties file in `bonita-home/engine-server/conf/tenants/`*``*.
    a.  Remove the comment flags from these lines:
    b.  Specify the relevant IP address and port number.
    c.  Optionally, to enable a Java client application to access the engine using CAS authentication, uncomment this line:

Configure Bonita BPM Engine and Tomcat for CAS {#conf_engine_tomcat}
----------------------------------------------

1.  The CAS implementation relies on JAAS, and is defined in the BonitaAuthentication module of the JAAS configuration file.
    Set the Java system property `java.security.auth.login.config` in the Tomcat startup script to point to the JAAS configuration file, `bonita-home/client/platform/conf/jaas.config`.
    For example, on Linux, edit `setenv.sh`, uncomment the line that defines `SECURITY_OPTS`, and
    insert the variable `SECURITY_OPTS` in the line `CATALINA_OPTS=..`.

    The `bonita-home/client/platform/conf/jaas.config` file contains the following (replace `ip_address:port` with the relevant IP addresses and port numbers, in two places):

    ` BonitaAuthentication-1 {   org.jasig.cas.client.jaas.CasLoginModule required     ticketValidatorClass="org.jasig.cas.client.validation.Cas20ServiceTicketValidator"     casServerUrlPrefix="http://ip_address:port/cas"     tolerance="20000"     service="http://ip_address:port/loginservice"     defaultRoles="admin,operator"     roleAttributeNames="memberOf,eduPersonAffiliation"     principalGroupName="CallerPrincipal"     roleGroupName="Roles"     cacheAssertions="true"     cacheTimeout="480"; };`{type="java"}

    The JAAS configuration file, `jaas.config`, is sorted by sets of authentication modules.
    For Bonita BPM, each set matches a tenant configuration and the name is prefixed with *BonitaAuthentication-``*.
    Make sure there is a set of authentication modules for each tenant in your platform.
    For each tenant, set the CAS service to point to the application login page and set `casServerUrlPrefix` to point to the CAS server.

    Note: The module name must be unique, BonitaAuthentication-1 in the above example.

2.  In the `CasLoginModule` configuration, check that the `principalGroupName` property is set to `CallerPrincipal`. This is required to retrieve the username from the Bonita application.
    Bonita BPM uses the CAS LoginModule in the JASIG implementation, so see the CAS LoginModule section of the [Jasig documentation](https://wiki.jasig.org/display/CASC/JAAS+Integration) for more information.
3.  Put the CAS client core jar (currently `cas-client-core-3.2.1.jar`) in the `/lib` folder.
4.  Put `commons-logging-1.1.jar` in the `/lib` folder.
5.  Update `bonita-tenant-sp-custom.properties`. If you edit this in `bonita-home/engine-server/conf/tenants/template/`, the settings will apply to all new tenants.
    For an existing tenant, edit the properties file in `bonita-home/engine-server/conf/tenants/`*``*.
    a.  Remove the comment flags from these lines:
    b.  Optionally, to enable a Java client application to access the engine using CAS autentication, uncomment this line:
    c.  Specify the relevant IP address and port number.

### Cluster considerations

If you are configuring Bonita BPM and Tomcat in a cluster environment for CAS, there are some extra steps to do:

1.  Copy `commons-logging-1.1.1.jar` from the `bonita.war` to `tomcat/lib`.
2.  Remove the `WEB-INF/lib/commons-logging-1.1.1.jar` file from the `bonita.war`.
3.  Remove the `tomcat/webapps/bonita/WEB-INF/lib/commons-logging-1.1.1.jar` file (if it is present).

Configure Bonita client for CAS {#conf_client}
-------------------------------

1.  For each tenant, edit `authenticationManager-config.properties` to enable the CASRemoteAuthenticationManager and its properties.
    If you edit this in `bonita-home/client/platform/tenant-template/conf/`, the settings will apply to all new tenants.
    For an existing tenant, edit the properties file in `bonita-home/client/tenants/`*``*.
    The service URL in the properties file must be the same as that in the JAAS file. The `authenticationManager-config.properties` will have the following content (specify the relevant IP address and ports):
    ` #auth.AuthenticationManager = org.bonitasoft.console.common.server.auth.impl.standard.StandardAuthenticationManagerImplExt #auth.AuthenticationManager = org.bonitasoft.console.common.server.auth.impl.oauth.OAuthAuthenticationManagerImplExt # OAuth.serviceProvider = LinkedIn # OAuth.consumerKey = ove2vcdjptar # OAuth.consumerSecret = vdaBrCmHvkgJoYz1 # OAuth.callbackURL = http://ip_address:port/loginservice auth.AuthenticationManager = org.bonitasoft.console.common.server.auth.impl.jaas.cas.CASRemoteAuthenticationManagerImpl Cas.serverUrlPrefix = http://ip_address:port/cas Cas.bonitaServiceURL = http://ip_address:port/bonita/loginservice logout.link.hidden=true`{type="xml"}
2.  Make sure that the default tenant id is the same on both the client side and server side.
    To do this, you can set the `platform.tenant.default.id` property in `bonita-home/client/platform/conf/platform-tenant-config.properties`.

Configure logout behaviour {#logout_option}
--------------------------

### Bonita BPM Portal

If you are using CAS, when users log out of Bonita BPM Portal, they log out of CAS. Therefore they are logged out of all applications that are using the CAS service. To avoid this, you can hide the logout option of the portal.
To do this, set the `logout.link.hidden=true` option in `authenticationManager-config.properties`.

If this option is set, when users navigate away from the Portal, they are still logged in to CAS.

### Bonita BPM Engine

By default, logging out from Bonita BPM Engine logs the user out of CAS. You can change this behavior by implementing your own Authentication Service.

Manage passwords {#passwords}
----------------

When you are using CAS, the password for a user is managed in your CAS system. However, when you create a user in Bonita BPM Portal, specifying a password is mandatory. This password is ignored.

LDAP synchronizer and CAS {#ldap}
-------------------------

If you are using an LDAP service and the [LDAP synchronizer](/ldap-synchronizer-3) to manage your user data, you can continue to do this and use CAS. The LDAP synchronizer user must be registered in CAS.
Alternatively, the LDAP synchronizer could be run with the tenant technical user, because this bypasses the SSO login.

We recommend that you use LDAP as your master source for information, synchronizing the relevant information with your CAS server.

Single sign-on with CAS using the REST API {#rest}
------------------------------------------

For detailed information about the procedure to install Restful access on your CAS SSO server, see the following links:

[CAS SSO RESTful API](http://jasig.github.io/cas/4.1.x/index.html)
[Bonita BPM REST API](/rest-api-overview-0)

<div class="alert alert-info">

<span class="glyphicon glyphicon-info-sign"></span> **Note:** All calls issued to get the TGT or ST are made to the CAS SSO server.

</div>

### Getting the Ticket Granting Ticket (TGT)

The Ticket Granting Ticket is an exposed resource. It has a unique URL.

#### **Request for a Ticket Granting Ticket Resource**

|                |                                                 |
|----------------|-------------------------------------------------|
| Request URL    | `http://www.your_cas_server_url/cas/v1/tickets` |
| Request Method | POST                                            |
| Form Data      | Username: walter.bates\                         
                  Password: bpm\                                   |

#### **Response for a Ticket Granting Ticket Resource**

+--------------------------------------+--------------------------------------+
| Response                             | 201 created                          |
|                                      |                                      |
|                                      | `Location: http://www.your_cas_serve |
|                                      | r_url/cas/v1/tickets/{TGT}`          |
+--------------------------------------+--------------------------------------+

Take the TGT response and paste it in the url of the ST request, below

### Getting the Service Ticket (ST)

#### **Request for a Service Ticket**

|                |                                                       |
|----------------|-------------------------------------------------------|
| Request URL    | `http://www.your_cas_server_url/cas/v1/tickets/{TGT}` |
| Request Method | POST                                                  |
| Form Data      | service={form encoded parameter for the service url}  |

#### **Response for a Service (ST)**

+--------------------------------------+--------------------------------------+
| Response                             | 200 OK                               |
|                                      |                                      |
|                                      | {ST}                                 |
+--------------------------------------+--------------------------------------+

Take the ST response and paste it in the url of the Bonita BPM Engine login request, below

### Logging into Bonita BPM Engine with Rest API using the service ticket

**Authentication to Bonita BPM Engine**

|                |                                                      |
|----------------|------------------------------------------------------|
| Request URL    | `bonita_server_url/loginservice?ticket={ST} `        |
| Request Method | GET                                                  |
| Form Data      | service={form encoded parameter for the service url} |

Limitation: The default AuthenticationFilter that manages CAS authentication applies only to the following pages:

-   /portal
-   /mobile/\*
-   /portal.js/\*
-   /apps/\*
-   /services/\*

If you require direct calls to the REST API to authenticate the user. you need to provide a custom Filter.

#### **Response for a Service (ST)**

|          |        |
|----------|--------|
| Response | 200 OK |

