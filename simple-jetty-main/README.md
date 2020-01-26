# Embedded Jetty Server for Google App Engine Standard with Java 11

This artifact provides a `Main` class to instantiate an HTTP server to run an
embedded web application `WAR` file.

## Install the dependency

This artifact is used as a dependency and must be installed locally:

    mvn install

## Using the dependency

Add the `simple-jetty-main` dependency to your project's `pom.xml`:

    <dependency>
      <groupId>com.example.appengine.demo</groupId>
      <artifactId>simple-jetty-main</artifactId>
      <version>1</version>
      <scope>provided</scope>
    </dependency>

On deployment, the App Engine runtime uploads files located in
`${build.directory}/appengine-staging`. Add the `maven-dependency-plugin` to
the build in order to copy dependencies to the correct folder:

    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-dependency-plugin</artifactId>
      <version>3.1.1</version>
      <executions>
        <execution>
          <id>copy</id>
          <phase>prepare-package</phase>
          <goals>
            <goal>copy-dependencies</goal>
          </goals>
          <configuration>
            <outputDirectory>
              ${project.build.directory}/appengine-staging
            </outputDirectory>
          </configuration>
        </execution>
      </executions>
    </plugin>

To use the dependency add the entrypoint to your `app.yaml` file. The
entrypoint field will start the Jetty server and load your `WAR` file.

    runtime: java11
    instance_class: F1
    entrypoint: 'java -cp "*" com.example.appengine.demo.jettymain.Main sample.war'
