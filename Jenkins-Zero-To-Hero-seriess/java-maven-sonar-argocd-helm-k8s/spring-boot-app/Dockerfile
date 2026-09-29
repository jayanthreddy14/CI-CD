FROM maven:3.9-eclipse-temurin-11 AS builder

WORKDIR /app

COPY pom.xml .
COPY src ./src

RUN mvn clean package -DskipTests

# Runtime stage
FROM adoptopenjdk/openjdk11:alpine-jre

WORKDIR /opt/app

COPY --from=builder /app/target/spring-boot-web.jar app.jar

ENTRYPOINT ["java","-jar","app.jar"]


# Base image: Starts with a Maven image that already has JDK 11 installed (maven:3.9-eclipse-temurin-11).

# WORKDIR /app: Sets the working directory inside the container to /app.

# COPY pom.xml .: Copies your Maven pom.xml file into /app.

# COPY src ./src: Copies your project’s source code into /app/src.

# RUN mvn clean package -DskipTests: Runs Maven inside the container to build your project.

# This creates a JAR file in /app/target/ (inside the container).

# Example: /app/target/spring-boot-web.jar.

# Base image: Uses a lightweight JRE image (adoptopenjdk/openjdk11:alpine-jre) for running the app.

# WORKDIR /opt/app: Sets the runtime working directory to /opt/app.

# COPY --from=builder: Copies the JAR built in the first stage (/app/target/spring-boot-web.jar) into this runtime image as /opt/app/app.jar.

# ENTRYPOINT: Defines the command to run when the container starts → java -jar app.jar.

# Maven builds your project and places the output in the target/ directory.

# By default, Maven names the JAR using the values in your pom.xml:

# artifactId → base name of the JAR

# version → appended unless you override it

# Example: if your pom.xml has

# xml
# <artifactId>spring-boot-web</artifactId>
# <version>1.0.0</version>
# then Maven will produce:

# Code
# target/spring-boot-web-1.0.0.jar
