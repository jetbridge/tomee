index-group=IDE
type=page
status=published
title=TomEE and Eclipse
~~~~~~
Notice:    Licensed to the Apache Software Foundation (ASF) under one
           or more contributor license agreements.  See the NOTICE file
           distributed with this work for additional information
           regarding copyright ownership.  The ASF licenses this file
           to you under the Apache License, Version 2.0 (the
           "License"); you may not use this file except in compliance
           with the License.  You may obtain a copy of the License at
           .
             http://www.apache.org/licenses/LICENSE-2.0
           .
           Unless required by applicable law or agreed to in writing,
           software distributed under the License is distributed on an
           "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
           KIND, either express or implied.  See the License for the
           specific language governing permissions and limitations
           under the License.

Using TomEE in Eclipse is pretty simple since it uses the existing Tomcat 7 server adaptor that comes
with Eclipse.

## Quick Start

An excellent instructional video with step-by-step instructions on how to install Eclipse, TomEE
and create/deploy your first project is available on YouTube here: [Getting Started with Apache TomEE][1] (Remember to select the 'Generate web.xml descriptor' checkbox during project setup. Otherwise the ejb does not get injected and you will get an error.).

1. Download and install both Apache TomEE and Eclipse.

1. Start Eclipse and from the main menu go to **File** - **New** - **Dynamic Web Project**

1. Enter a new project name

1. In the **Target Runtime** section click on the **New Runtime** button.

1. Pick **Apache Tomcat v7.0** and click Next

1. Change the **Name** field to *TomEE* to indicate that this is a TomEE server rather than a Tomcat server.

1. Set the **Tomcat installation directory** by clicking the **Browse** button and selecting the folder
where you extracted TomEE

1. Click **Finish** to return to the New Project dialog

1. Click **Finish** to complete creating your new Project

1. When you're ready to deploy your project, right-click your project and select Run As - Run On Server

1. Make sure that the *TomEE* environment is selected in the **Server runtime environment**

1. On the **Run on Server** dialog, click the **Always use this server when running this project** checkbox

1. Click **Finish** - Eclipse will start TomEE and deploy your project

## Advanced installation

1. In Eclipse, click on the **Servers** tab, right click and select New - Server.

1. Select **Apache - Tomcat v7.0 Server** and click **Next**

1. Set the **Tomcat installation directory** by clicking the **Browse** button and selecting the folder
where you extracted TomEE

1. Add your webapp to the server. Click **Finish**.

1. In the **Servers** tab, double click on your server to open up the **Overview** page.
Click on the **Modules** tab

1. Click **Add External Web Module**. In the **Add Web Module** dialog, for document base, browse
to `<TomEE>/webapps/tomee`. Set **Path:** to `/tomee`. Uncheck **Auto reloading enabled**. Click OK.

1. Return to the **Overview** tab for the server.

1. Deselect the **Modules auto reload by default** checkbox.

1. If you do not want Eclipse to take control of your TomEE installation, select **Use Workspace Metadata**
under **Server Locations**. Please review the *Workspace Metadata Installation* section below for additional steps
in this scenario. Otherwise, select **Use Tomcat Installation**

1. Click the Save button in Eclipse so the server configuration gets saved.

1. Click on your webapp project, then select Project - clean. Hit OK. This will cause
Eclipse to clean and rebuild

1. In the **Servers** tab, right click on the server and select **Publish**.

1. Start the server.

### Workspace Metadata Installation
If you used **Use Workspace Metadata** in the **Server Locations** definition of the TomEE server,
you will have to add TomEE specific configuration files to your Servers project in your workspace
in case you need to change system properties or add containers / resources and have those reflected in the
server running under Eclipse.

1. Locate your TomEE server configuration in Workspace / Servers

1. Right-click the server project and select **Import**

1. Select General - File System and click **Next**

1. Browse to your `<TomEE>/conf` folder

1. Select the following files for import: `logging.properties`, `system.properties` and `tomee.xml`

1. Click **Finish** to import the files.

1. In the **Servers** tab, right-click the TomEE server and select **Publish** to publish the files to the
Eclipse metadata folder

If you need to modify system properties or change your TomEE resources, containers, etc., you now need to make
those changes in the Workspace / Servers / `<`Your Server`>` location and publish them to the server for
the changes to take effect.

### JSP Hot Deployment

If jsp changes are not being hot deployed then this is because the jsp servlet is set to development=false in the web.xml file. To allow jsp hot deployment alter this value to true and restart Tomee.

This is the relevant snippet of the web.xml file.

    <servlet>
        <servlet-name>jsp</servlet-name>
        <servlet-class>org.apache.jasper.servlet.JspServlet</servlet-class>
        ....
        <init-param>
            <param-name>development</param-name>
            <param-value>true</param-value>
        </init-param>
        ....
    </servlet>

## How to use JULI for TomEE in WTP?

It seems that WTP doesn't manage correctly Tomcat logging configuration (which needs to be done through
system properties). A quick workaround is to add these properties manually:

    -Djava.util.logging.config.file="<tomee>/conf/logging.properties"
    -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager

More information on: http://wiki.eclipse.org/WTP_Tomcat_FAQ#How_do_I_enable_the_JULI_logging_in_a_Tomcat_5.5_Server_instance.3F


  [1]: http://www.youtube.com/watch?v=Lr8pxEACVRI "Getting Started with Apache TomEE"
