# Using Terminal from eclipse
```
USe tcf terminal plugin in eclipse to use command line terminal and execute maven jobs.
```

```
Seeing the content of parent pom along with child pom
  - mvn help:effective-pom
```


# Standard Maven directory structure
```
https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html

src/main/java
    --- java source code
    
src/main/resources
    --- configuration files like log4j.xml,.properties, etc
    
src/test/java
    --- jnuit test classess
    
src/test/resources
    --- test resources
    
To override these standard locations

update build tag in pom

eg 

<build>
  <sourceDirectory>src/nonstandard/java</sourceDirectory>
  <directory>myTarget</directory>
</build>

```
# Profiles
```
  <profiles>
		<profile>
      <id>production_id</id>
      <build>
      	<directory>production</directory>
      </build>
    </profile>
    
    
    mvn -Pproduction_id package  --- runnuing through command line
    
    
    other way through activation
    
    <profiles>
		<profile>
      <id>production</id>
      <activation>
      	<!-- Use any of the below activation types -->
      	<!-->activeByDefault></activeByDefault -->
      	<!--file></file-->
      	<!--jdk></jdk-->
      	<!--os></os-->
      	<property>
      		<name>env.JAVA_HOME</name>
      		<value>/opt/java</value>
      	</property>
      </activation>
      <build>
      	<directory>production</directory>
      </build>
    </profile>

```
# Archetype Plugin -- use to create project template from exisiting templates
```
	mvn archetype:generate -- here archetype is the plugin and generate is the action
	
	or
	
	mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.3
```
# Dependency Management
```
search.maven.org - search engine to search artefacts

mvn dependency:copy-dependencies

Above command will copy all the dependencies specified in pom.xml in target/dependency folder and local .m2 repo

* Transitive Dependency
	 A -> B -> C
	 
A depends on B and B depends on C , so B & C are transitive dependencies for A
	How to resolve transitive dependency conflict when multiple dependencies pull same transiticvve dependecies, but differenct version
	
<dependencies>
    <dependency>
      <groupId>group-a</groupId>
      <artifactId>artifact-a</artifactId>
      <version>1.0</version>
      <exclusions>
        <exclusion>
          <groupId>group-c</groupId>
          <artifactId>excluded-artifact</artifactId>
        </exclusion>
      </exclusions>
    </dependency>
 </dependencies>
 
 Note: In case of conflict, maven will use the latest version of transitive dependency.
	
```
#  Remote repositories - collection of dependencies and plugins
```
configure in settings.xml
-- Create a profile

<profiles>
 <profile>
      <id>production</id>
      <repositories>
      	<repository>
      		<id>spring_repo</id>
      		<url>http://repo.spring.io.release</url>
      	</repository>
      </repositories>
 </profile>
</profiles>
  
  <activeProfiles>
  <activeProfile>production</activeProfile>
  </activeProfiles>
```
# Dependency Scope
```
	1 compile (default)
	2 test
	3 provided (will be provided eg by JDK - Dependency will be available at build time, but would be provided at run time)
	4 runtime (Specify a dependency is needed at run time, but not required at build time)
	5 import (Not used often)
	6 system (Not recommended. locate the dependency from the machine path)
	
<dependencies>
    <dependency>
      <groupId>group-c</groupId>
      <artifactId>artifact-b</artifactId>
      <version>1.0</version>
      <type>war</type>
      <scope>runtime</scope>
    </dependency>
 <dependencies>
```
# Manually running a Plugin and it's goal
```
  - mvn -X (flag for debug information)
  - mvn compiler:compile
```

```
To see details of specific plugin eg compiler plugin
  - -D is taking name if the specific plugin
 - mvn help:describe -Dplugin=compiler
 
To see details of specific plugin goal eg compile goal of compiler plugin: It will also show all properties associated with the plugin goal
  - mvn help:describe -Dcmd=compiler:compile -Ddetail
  
To specify property for a plugin goal through command line
 
 -mvn compiler:compile -Dmaven.compiler.verbose=true
 
To set property for a plugin goal through pom definition eg verbose property

<build>
<pluginManagement>
  <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.2</version>
        <configguration>
          <verbose>true</verbose>
        </configguration>
      </plugin>        
  </plugins>
</pluginManagement>
</build> 
```

```
To see 'goal' details of compiler plugin
  - Here help is a goal of compiler plugin
  - -D is used to passed parameters

mvn compiler:help -Ddetail=true -Dgoal=compile
```

```
Building custom plugins
  Below command will generate project to create custom plugin
mvn archetype:create -DgroupId= -DartifactId= -DarchetypeArtifactId=maven-archetype-mojo -DarchetypeGroupId=org.apache.maven.archetypes

```
