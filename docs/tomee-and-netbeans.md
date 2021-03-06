index-group=IDE
type=page
status=published
title=TomEE and NetBeans
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
		   
There is some great information over at [Geertjan's Blog](https://blogs.oracle.com/geertjan/entry/tomee_apache_cxf_and_maven) on how to hit the ground running with Netbeans, CXF and Maven. Geertjan is a Netbeans evangelist and has an incredible insight into everything Netbeans.

**WORKAROUND**: There is a known issue with Netbeans 8 and TomEE detection that currently requires the following workaround:

Netbeans 8 has a bug in which it fails to find the **tomee-common-[version].jar** in the **[TomEE]/lib** directory. The solution is to simply rename the jar file to an older version.

For example, you have **[TomEE]/lib/tomee-common-1.6.0.2.jar** or **[TomEE]/lib/tomee-common-1.7.1.jar**. Rename these files to **[TomEE]/lib/tomee-common-1.6.0.jar**

This should resolve the detection issue and will not break your installation - Be sure to document the change for yourself as a reminder.

###Quickstart
Check out this video on [How to Consume REST in a Java Client ](https://www.youtube.com/watch?v=HISV7eagogI)

You can download Netbeans 8 here: [https://netbeans.org/community/releases/80/](https://netbeans.org/community/releases/80/)

Here is a quick run through on how to set up TomEE. We will use one of the existing examples for this demo. Let's import it.  

  ![Subversion Checkout][1]
  ![Subversion URL][2]
  ![Local Project][3]
  ![alt text][4]

Click 'Open Project'.  

  ![alt text][5]
  ![alt text][6]

It's time to add our local TomEE server. Click 'Tools' and then 'Servers'.  

  ![alt text][7]

Select 'Apache Tomcat'.  

  ![alt text][8]
  
Select your local TomEE directory.  

  ![alt text][9]
  ![alt text][10]
  ![alt text][11]

It's time to run it. Click the play button.  

  ![alt text][12]

Select 'Apache Tomcat'.  

  ![alt text][13]

Give it some time. It's building your application.  

  ![alt text][14]

Done. Your server is up and running.  

  ![alt text][15]
  ![alt text][16]


  [1]: http://people.apache.org/~tveronezi/tomee/tomee_site/netbeans_integration/windows8_01.png
  [2]: http://people.apache.org/~tveronezi/tomee/tomee_site/netbeans_integration/windows8_02.png
  [3]: http://people.apache.org/~tveronezi/tomee/tomee_site/netbeans_integration/windows8_03.png
  [4]: http://people.apache.org/~tveronezi/tomee/tomee_site/netbeans_integration/windows8_04.png
  [5]: http://people.apache.org/~tveronezi/tomee/tomee_site/netbeans_integration/windows8_05.png
  [6]: http://people.apache.org/~tveronezi/tomee/tomee_site/netbeans_integration/windows8_06.png
  [7]: http://people.apache.org/~tveronezi/tomee/tomee_site/netbeans_integration/windows8_07.png
  [8]: http://people.apache.org/~tveronezi/tomee/tomee_site/netbeans_integration/windows8_08.png
  [9]: http://people.apache.org/~tveronezi/tomee/tomee_site/netbeans_integration/windows8_09.png
  [10]: http://people.apache.org/~tveronezi/tomee/tomee_site/netbeans_integration/windows8_10.png
  [11]: http://people.apache.org/~tveronezi/tomee/tomee_site/netbeans_integration/windows8_11.png
  [12]: http://people.apache.org/~tveronezi/tomee/tomee_site/netbeans_integration/windows8_12.png
  [13]: http://people.apache.org/~tveronezi/tomee/tomee_site/netbeans_integration/windows8_13.png
  [14]: http://people.apache.org/~tveronezi/tomee/tomee_site/netbeans_integration/windows8_14.png
  [15]: http://people.apache.org/~tveronezi/tomee/tomee_site/netbeans_integration/windows8_15.png
  [16]: http://people.apache.org/~tveronezi/tomee/tomee_site/netbeans_integration/windows8_16.png
