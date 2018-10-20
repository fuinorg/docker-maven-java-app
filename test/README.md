# docker-maven-java-app-test
A dockerized Java standalone application with Maven build / TEST

This is an integration test project (See [HelloWorldIT.java](src/test/java/org/fuin/examples/dmjapp/HelloWorldIT.java)). The Maven [pom.xml](pom.xml) starts a Docker container and the integration test verifies that the necessary files were written to the target directory. The image used for the container was previously built by the [docker-maven-java-app-main](../main) module.