# docker-maven-java-app-main
A dockerized Java standalone application with Maven build / MAIN

This is the main command line Java application (See [HelloWorld.java](src/main/java/org/fuin/examples/dmjapp/HelloWorld.java)). The Maven [pom.xml](pom.xml) creates a Docker image that simply starts the Java application and terminates after the program ended. The module [docker-maven-java-app-test](../test) picks up the created image and starts an integration test using it.
  