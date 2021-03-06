index-group=Unrevised
type=page
status=published
title=JavaAgent with Maven Surefire
~~~~~~
<a name="JavaAgentwithMavenSurefire-Maven2"></a>
##  Maven2

In maven2 you can enable the javaagent for your tests by adding this to
your pom.xml file:


    <build>
      <plugins>
        <!-- this configures the surefire plugin to run your tests with the
javaagent enabled -->
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-surefire-plugin</artifactId>
          <configuration>
    	<forkMode>pertest</forkMode>
           
<argLine>-javaagent:${basedir}/target/openejb-javaagent-3.0.jar</argLine>
    	<workingDirectory>${basedir}/target</workingDirectory>
          </configuration>
        </plugin>
    
        <!-- this tells maven to copy the openejb-javaagent jar into your
target/ directory -->
        <!-- where surefire can see it -->
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-dependency-plugin</artifactId>
          <executions>
    	<execution>
    	  <id>copy</id>
    	  <phase>process-resources</phase>
    	  <goals>
    	    <goal>copy</goal>
    	  </goals>
    	  <configuration>
    	    <artifactItems>
    	      <artifactItem>
    		<groupId>org.apache.openejb</groupId>
    		<artifactId>openejb-javaagent</artifactId>
    		<version>3.0</version>
    	       
<outputDirectory>${project.build.directory}</outputDirectory>
    	      </artifactItem>
    	    </artifactItems>
    	  </configuration>
    	</execution>
          </executions>
        </plugin>
    
      </plugins>
    </build>
