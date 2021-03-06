index-group=TomEE Maven Plugin
type=page
status=published
title=TomEE Embedded Maven Plugin
~~~~~~

[TomEE Maven Plugin](tomee-maven-plugin.html) provides a nice way to run "as in production" a server fully configured
keeping the configuration in the project (easiness of sharing between team members). However for modern web development
the fact to run the "exploded war" prevents to develop web resources in place. TomEE embedded maven plugin
solves it directly allowing to directly deploy the war project in place using "classpath as war" option.

It also allows to use a flat classpath deployment which is often use with microservices.

<h2><a name="tomee-embedded:run"></a>tomee-embedded:run</h2>
      
<p><b>Full name</b>:</p>
      
<p>org.apache.tomee.maven:tomee-embedded-maven-plugin:7.0.0-M1:run</p>
      
<p><b>Description</b>:</p>
      
<div>Run an Embedded TomEE.</div>
      
<p><b>Attributes</b>:</p>
      
<ul>
        
<li>Requires a Maven project to be executed.</li>
        
<li>Requires dependency resolution of artifacts in scope: <tt>runtime+system</tt>.</li>
        
<li>Requires dependency collection of artifacts in scope: <tt>runtime</tt>.</li>
      </ul>
      
<div class="section">
<h3><a name="Optional_Parameters"></a>Optional Parameters</h3>
        
<table class="mdtable">
          
<tr class="a">
            
<th>Name</th>
            
<th>Type</th>
            
<th>Since</th>
            
<th>Description</th>
          </tr>
          
<tr class="b">
            
<td><b><a href="#ajpPort">ajpPort</a></b></td>
            
<td><tt>int</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>8009</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.ajp</tt>.</td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#applicationCopyFolder">applicationCopyFolder</a></b></td>
            
<td><tt>File</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>${project.build.directory}/tomee-embedded/applications</tt>.<br /><b>User property is</b>: <tt>tomee-plugin.application-copy</tt>.</td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#applicationScopes">applicationScopes</a></b></td>
            
<td><tt>List</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /></td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#applications">applications</a></b></td>
            
<td><tt>List</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /></td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#classpathAsWar">classpathAsWar</a></b></td>
            
<td><tt>boolean</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>false</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.classpathAsWar</tt>.</td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#clientAuth">clientAuth</a></b></td>
            
<td><tt>String</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>User property is</b>: <tt>tomee-embedded-plugin.clientAuth</tt>.</td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#containerProperties">containerProperties</a></b></td>
            
<td><tt>Map</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /></td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#context">context</a></b></td>
            
<td><tt>String</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>User property is</b>: <tt>tomee-embedded-plugin.context</tt>.</td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#dir">dir</a></b></td>
            
<td><tt>String</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>${project.build.directory}/apache-tomee-embedded</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.lib</tt>.</td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#docBase">docBase</a></b></td>
            
<td><tt>File</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>${project.basedir}/src/main/webapp</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.docBase</tt>.</td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#forceJspDevelopment">forceJspDevelopment</a></b></td>
            
<td><tt>boolean</tt></td>
            
<td><tt>-</tt></td>
            
<td>force webapp to be reloadable<br /><b>Default value is</b>: <tt>true</tt>.<br /><b>User property is</b>: <tt>tomee-plugin.jsp-development</tt>.</td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#host">host</a></b></td>
            
<td><tt>String</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>localhost</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.host</tt>.</td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#httpPort">httpPort</a></b></td>
            
<td><tt>int</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>8080</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.http</tt>.</td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#httpsPort">httpsPort</a></b></td>
            
<td><tt>int</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>8443</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.httpsPort</tt>.</td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#inlinedServerXml">inlinedServerXml</a></b></td>
            
<td><tt>PlexusConfiguration</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /></td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#inlinedTomEEXml">inlinedTomEEXml</a></b></td>
            
<td><tt>PlexusConfiguration</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /></td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#keepServerXmlAsThis">keepServerXmlAsThis</a></b></td>
            
<td><tt>boolean</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>false</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.keepServerXmlAsThis</tt>.</td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#keyAlias">keyAlias</a></b></td>
            
<td><tt>String</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>User property is</b>: <tt>tomee-embedded-plugin.keyAlias</tt>.</td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#keystoreFile">keystoreFile</a></b></td>
            
<td><tt>String</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>User property is</b>: <tt>tomee-embedded-plugin.keystoreFile</tt>.</td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#keystorePass">keystorePass</a></b></td>
            
<td><tt>String</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>User property is</b>: <tt>tomee-embedded-plugin.keystorePass</tt>.</td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#keystoreType">keystoreType</a></b></td>
            
<td><tt>String</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>JKS</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.keystoreType</tt>.</td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#mavenLog">mavenLog</a></b></td>
            
<td><tt>boolean</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>true</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.mavenLog</tt>.</td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#modules">modules</a></b></td>
            
<td><tt>List</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>${project.build.outputDirectory}</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.modules</tt>.</td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#packaging">packaging</a></b></td>
            
<td><tt>String</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>${project.packaging}</tt>.<br /></td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#quickSession">quickSession</a></b></td>
            
<td><tt>boolean</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>true</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.quickSession</tt>.</td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#roles">roles</a></b></td>
            
<td><tt>Map</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /></td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#serverXml">serverXml</a></b></td>
            
<td><tt>File</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /></td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#skipCurrentProject">skipCurrentProject</a></b></td>
            
<td><tt>boolean</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>false</tt>.<br /><b>User property is</b>: <tt>tomee-plugin.skip-current-project</tt>.</td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#skipHttp">skipHttp</a></b></td>
            
<td><tt>boolean</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>false</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.skipHttp</tt>.</td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#ssl">ssl</a></b></td>
            
<td><tt>boolean</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>false</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.ssl</tt>.</td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#sslProtocol">sslProtocol</a></b></td>
            
<td><tt>String</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>User property is</b>: <tt>tomee-embedded-plugin.sslProtocol</tt>.</td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#stopPort">stopPort</a></b></td>
            
<td><tt>int</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>8005</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.stop</tt>.</td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#useProjectClasspath">useProjectClasspath</a></b></td>
            
<td><tt>boolean</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>true</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.useProjectClasspath</tt>.</td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#users">users</a></b></td>
            
<td><tt>Map</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /></td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#warFile">warFile</a></b></td>
            
<td><tt>File</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>${project.build.directory}/${project.build.finalName}</tt>.<br /></td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#webResourceCached">webResourceCached</a></b></td>
            
<td><tt>boolean</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>true</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.webResourceCached</tt>.</td>
          </tr>
          
<tr class="b">
            
<td><b><a href="#withEjbRemote">withEjbRemote</a></b></td>
            
<td><tt>boolean</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>false</tt>.<br /><b>User property is</b>: <tt>tomee-embedded-plugin.withEjbRemote</tt>.</td>
          </tr>
          
<tr class="a">
            
<td><b><a href="#workDir">workDir</a></b></td>
            
<td><tt>File</tt></td>
            
<td><tt>-</tt></td>
            
<td>(no description)<br /><b>Default value is</b>: <tt>${project.build.directory}/tomee-embedded-work</tt>.<br /><b>User property is</b>: <tt>tomee-plugin.work</tt>.</td>
          </tr>
        </table>
      </div>
      
<div class="section">
<h3><a name="Parameter_Details"></a>Parameter Details</h3>
        
<p><b><a name="ajpPort">ajpPort</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>int</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.ajp</tt></li>
          
<li><b>Default</b>: <tt>8009</tt></li>
        </ul><hr />
<p><b><a name="applicationCopyFolder">applicationCopyFolder</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.io.File</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-plugin.application-copy</tt></li>
          
<li><b>Default</b>: <tt>${project.build.directory}/tomee-embedded/applications</tt></li>
        </ul><hr />
<p><b><a name="applicationScopes">applicationScopes</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.util.List</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
        </ul><hr />
<p><b><a name="applications">applications</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.util.List</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
        </ul><hr />
<p><b><a name="classpathAsWar">classpathAsWar</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>boolean</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.classpathAsWar</tt></li>
          
<li><b>Default</b>: <tt>false</tt></li>
        </ul><hr />
<p><b><a name="clientAuth">clientAuth</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.lang.String</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.clientAuth</tt></li>
        </ul><hr />
<p><b><a name="containerProperties">containerProperties</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.util.Map</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
        </ul><hr />
<p><b><a name="context">context</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.lang.String</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.context</tt></li>
        </ul><hr />
<p><b><a name="dir">dir</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.lang.String</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.lib</tt></li>
          
<li><b>Default</b>: <tt>${project.build.directory}/apache-tomee-embedded</tt></li>
        </ul><hr />
<p><b><a name="docBase">docBase</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.io.File</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.docBase</tt></li>
          
<li><b>Default</b>: <tt>${project.basedir}/src/main/webapp</tt></li>
        </ul><hr />
<p><b><a name="forceJspDevelopment">forceJspDevelopment</a>:</b></p>
        
<div>force webapp to be reloadable</div>
        
<ul>
          
<li><b>Type</b>: <tt>boolean</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-plugin.jsp-development</tt></li>
          
<li><b>Default</b>: <tt>true</tt></li>
        </ul><hr />
<p><b><a name="host">host</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.lang.String</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.host</tt></li>
          
<li><b>Default</b>: <tt>localhost</tt></li>
        </ul><hr />
<p><b><a name="httpPort">httpPort</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>int</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.http</tt></li>
          
<li><b>Default</b>: <tt>8080</tt></li>
        </ul><hr />
<p><b><a name="httpsPort">httpsPort</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>int</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.httpsPort</tt></li>
          
<li><b>Default</b>: <tt>8443</tt></li>
        </ul><hr />
<p><b><a name="inlinedServerXml">inlinedServerXml</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>org.codehaus.plexus.configuration.PlexusConfiguration</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
        </ul><hr />
<p><b><a name="inlinedTomEEXml">inlinedTomEEXml</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>org.codehaus.plexus.configuration.PlexusConfiguration</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
        </ul><hr />
<p><b><a name="keepServerXmlAsThis">keepServerXmlAsThis</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>boolean</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.keepServerXmlAsThis</tt></li>
          
<li><b>Default</b>: <tt>false</tt></li>
        </ul><hr />
<p><b><a name="keyAlias">keyAlias</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.lang.String</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.keyAlias</tt></li>
        </ul><hr />
<p><b><a name="keystoreFile">keystoreFile</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.lang.String</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.keystoreFile</tt></li>
        </ul><hr />
<p><b><a name="keystorePass">keystorePass</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.lang.String</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.keystorePass</tt></li>
        </ul><hr />
<p><b><a name="keystoreType">keystoreType</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.lang.String</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.keystoreType</tt></li>
          
<li><b>Default</b>: <tt>JKS</tt></li>
        </ul><hr />
<p><b><a name="mavenLog">mavenLog</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>boolean</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.mavenLog</tt></li>
          
<li><b>Default</b>: <tt>true</tt></li>
        </ul><hr />
<p><b><a name="modules">modules</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.util.List</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.modules</tt></li>
          
<li><b>Default</b>: <tt>${project.build.outputDirectory}</tt></li>
        </ul><hr />
<p><b><a name="packaging">packaging</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.lang.String</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>Default</b>: <tt>${project.packaging}</tt></li>
        </ul><hr />
<p><b><a name="quickSession">quickSession</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>boolean</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.quickSession</tt></li>
          
<li><b>Default</b>: <tt>true</tt></li>
        </ul><hr />
<p><b><a name="roles">roles</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.util.Map</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
        </ul><hr />
<p><b><a name="serverXml">serverXml</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.io.File</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
        </ul><hr />
<p><b><a name="skipCurrentProject">skipCurrentProject</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>boolean</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-plugin.skip-current-project</tt></li>
          
<li><b>Default</b>: <tt>false</tt></li>
        </ul><hr />
<p><b><a name="skipHttp">skipHttp</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>boolean</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.skipHttp</tt></li>
          
<li><b>Default</b>: <tt>false</tt></li>
        </ul><hr />
<p><b><a name="ssl">ssl</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>boolean</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.ssl</tt></li>
          
<li><b>Default</b>: <tt>false</tt></li>
        </ul><hr />
<p><b><a name="sslProtocol">sslProtocol</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.lang.String</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.sslProtocol</tt></li>
        </ul><hr />
<p><b><a name="stopPort">stopPort</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>int</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.stop</tt></li>
          
<li><b>Default</b>: <tt>8005</tt></li>
        </ul><hr />
<p><b><a name="useProjectClasspath">useProjectClasspath</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>boolean</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.useProjectClasspath</tt></li>
          
<li><b>Default</b>: <tt>true</tt></li>
        </ul><hr />
<p><b><a name="users">users</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.util.Map</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
        </ul><hr />
<p><b><a name="warFile">warFile</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.io.File</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>Default</b>: <tt>${project.build.directory}/${project.build.finalName}</tt></li>
        </ul><hr />
<p><b><a name="webResourceCached">webResourceCached</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>boolean</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.webResourceCached</tt></li>
          
<li><b>Default</b>: <tt>true</tt></li>
        </ul><hr />
<p><b><a name="withEjbRemote">withEjbRemote</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>boolean</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-embedded-plugin.withEjbRemote</tt></li>
          
<li><b>Default</b>: <tt>false</tt></li>
        </ul><hr />
<p><b><a name="workDir">workDir</a>:</b></p>
        
<div>(no description)</div>
        
<ul>
          
<li><b>Type</b>: <tt>java.io.File</tt></li>
          
<li><b>Required</b>: <tt>No</tt></li>
          
<li><b>User Property</b>: <tt>tomee-plugin.work</tt></li>
          
<li><b>Default</b>: <tt>${project.build.directory}/tomee-embedded-work</tt></li>
        </ul>
      </div>
  
