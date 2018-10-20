# docker-maven-java-app
A dockerized Java standalone application with Maven build

## What is this?
This project shows how to automate the creation of a Docker image with a Java command line application using Maven.

- Creates a docker image that runs a Java [main method](main/src/main/java/org/fuin/examples/dmjapp/HelloWorld.java) on start of the container and terminates after the program is finished
  - Example application that writes the environment variables from the Docker container to a test file.
  - Also outputs some log statements
- Uses the fabric8 [maven-docker-plugin](https://github.com/fabric8io/docker-maven-plugin)
  - Build the image
  - Start a container for an [integration test](test/src/test/java/org/fuin/examples/dmjapp/HelloWorldIT.java)
  - Has a plain [Dockerfile](main/src/main/docker/Dockerfile) to configure the image
  - Uses the [maven-dependency-plugin](https://maven.apache.org/plugins/maven-dependency-plugin/) to collect the JARs needed for the application
  - Uses an [assembly.xml](main/src/main/assembly.xml) file to copy all files to the image 
- Mounts a directory in the container as a volume to a host directory
  - Uses the [system-maven-plugin](https://github.com/fuinorg/system-maven-plugin) to set the current user/group for the mount  
- Uses [logback](https://logback.qos.ch/) for logging
  - Creates a "logback.xml" configuration automatically if it does not exist. This allows a user to configure the log level for multiple runs of the container. 
  - Log file is created in the mounted directory of the host.
- Multi module Maven project with parent (this directory), [main](main/) application and [test](test/) project

## Build
Checkout the project and build it using the [Takari Maven Wrapper](https://github.com/takari/maven-wrapper).

Inside the root directory of the project type the following:

```
./mvnw clean verify
```

If this was successful you should see something like:

```
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary:
[INFO] 
[INFO] docker-maven-java-app-parent 0.1.0-SNAPSHOT ........ SUCCESS [  0.127 s]
[INFO] docker-maven-java-app-main ......................... SUCCESS [  6.009 s]
[INFO] docker-maven-java-app-test 0.1.0-SNAPSHOT .......... SUCCESS [  2.064 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
```

Verify that a Docker image was created:

```
docker images
```

This should display something like:

```
REPOSITORY                        TAG
fuinorg/docker-maven-java-app     0.1.0-SNAPSHOT
fuinorg/docker-maven-java-app     latest
```

Now run the application by 

```
docker run \
--volume "$(pwd)"/data:/usr/src/docker-maven-java-app/data \
--user $UID:$UID \
--env my_var=abc123 \
fuinorg/docker-maven-java-app
```

After running the container there should be three files in the [data](data) dircetory of the project:

```
hello-world.log
hello-world.txt
logback.xml
```
