# Java Machine

This folder contains instructions to set up a **containerized environment for running Java programs**.

## Usage

### Starting the Container

1. **Navigate to this folder**  
   Open your terminal and go to the folder containing the `docker-compose.yml` file.

2. **Run the command**:

        docker-compose up

**Explanation**: This command will download the necessary images, start the container, and run the Java application defined in `Main.java`.

### Restarting the Application

If you want to test your application with new changes, you need to stop and rebuild the container.

1. **Stop the containers and remove images**:

        docker-compose down --rmi all

## Explanation

### Dockerfile

The `Dockerfile` is used to build the Java application image.

```Dockerfile
FROM openjdk:17-jdk-alpine

RUN apk add --no-cache bash

WORKDIR /app

COPY ./src /app/src

CMD ["java", "src/Main.java"]
```

**Key Points**:

* `FROM openjdk:17-jdk-alpine`: Uses a lightweight OpenJDK 17 image.
* `RUN apk add --no-cache bash`: Installs `bash` in the container.
* `WORKDIR /app`: Sets the working directory inside the container.
* `COPY ./src /app/src`: Copies your source code into the container.
* `CMD ["java", "src/Main.java"]`: Command to run the Java application.

---

### docker-compose.yml

The `docker-compose.yml` file defines how to run the container.

```yaml
services:
  java:
    build: .
    container_name: java-app
    stdin_open: true
    tty: true
```

**Key Points**:

* `services`: Defines the services that run inside the container.
* `java`: Service name.
* `build: .`: Builds the image from the Dockerfile in the current directory.
* `container_name: java-app`: Assigns a name to the container.
* `stdin_open: true` and `tty: true`: Allow interactive console access inside the container.