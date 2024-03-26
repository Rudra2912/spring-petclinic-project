# Project Documentation - PetClinic

## Setup Guide

### Pre requisites

1)	[Git](https://git-scm.com/)
2)	[Java 17](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html)
3)	[Maven 3.9.6](https://maven.apache.org/download.cgi)
4)	[Jenkins](https://www.jenkins.io/download)
5)	[SonarQube](https://www.sonarsource.com/products/sonarqube/downloads/) 
6)	[Docker Desktop](https://www.docker.com/products/docker-desktop/)
7)	[VS Code](https://code.visualstudio.com/)
   
## Steps to run the project

There are 2 ways in which this project can be up and running

1)	Local Setup
2)	Using Jenkins
   
## Method 1 – Local Setup

1)	Clone the repo in your local machine
```bash
https://github.com/logicopslab/spring-petclinic-project.git
```
2)	Go inside spring-petclinic-project folder
```bash
spring-petclinic-project
```
3)	Open a command prompt with the same location and run
```bash
mvn package
```
4)	Once successful it will produce a jar file

![Screenshot 2024-03-25 211216](https://github.com/logicopslab/spring-petclinic-project/assets/82759985/583d3450-81d0-46a6-a53b-e2efb0689674)

6)	Run it by 
```bash
java -jar spring-petclinic-3.2.0-SNAPSHOT.jar
```
It will be up and running in some time.

![Screenshot 2024-03-25 211452](https://github.com/logicopslab/spring-petclinic-project/assets/82759985/39485d22-3a5b-4140-aa39-934ac284f6f6)

## Method 2 – Using Jenkins
1)	Install Jenkins, do the basic setup, once it is up and running.
2)	Create a new pipeline, use [Jenkinsfile](https://github.com/logicopslab/spring-petclinic-project/blob/main/Jenkinsfile)

![Screenshot 2024-03-25 211703](https://github.com/logicopslab/spring-petclinic-project/assets/82759985/9e4fd2d5-bd05-4bcd-8cb0-5e6deed9accf)

3)	Install Docker desktop
   
4)	Install SonarQube, do the basic setup. Run it on unused port. Once it is up and running, kick off the build in Jenkins.

5)	Make sure the build is passed.
![Screenshot 2024-03-26 225820](https://github.com/logicopslab/spring-petclinic-project/assets/82759985/5df6a97e-0f36-45d9-ab1a-0e17647aec55)

6)	Once the project runs with SonarQube, it will show these results
![Screenshot 2024-03-25 213729](https://github.com/logicopslab/spring-petclinic-project/assets/82759985/8d5989e2-1e55-411a-957e-d6b31517e791)


## Jenkinsfile explanation

Link - https://github.com/logicopslab/spring-petclinic-project/blob/main/Jenkinsfile

1.	Code Checkout:This stage uses the git command to clone the code from the main branch of the https://github.com/logicopslab/spring-petclinic-project.git repository.
2.	Clean and Install:This stage sets up the environment for Maven by defining the MAVEN_HOME variable pointing to the Maven installation directory (C:/Installers/apache-maven-3.9.6). 
It then updates the system path (PATH) to include the Maven binaries.
Finally, it uses a bat command (for Windows batch script execution) to run mvn clean install. This cleans any previously built artifacts and installs the project's dependencies.
3.	Build: This stage simply runs mvn package using a bat command. This packages the application into a distributable artifact (e.g., JAR file).
4.	Run Tests: This stage executes the tests using mvn test through a bat command. This verifies the functionality of the application.
5.	Code Quality (SonarQube): It defines a step using the withSonarQubeEnv function to connect to a SonarQube server named "SonarQube Server".
It then tries to run mvn sonar:sonar with a bat command, which is used for code quality analysis with SonarQube.
6.	Dockerizing (Placeholder – Working on it): Explained Separately
7.	Pushing to Artifactory (Placeholder – Working on it):
Similar to the previous stage, this is a placeholder with an echo statement "Pushing". This suggests the pipeline might eventually deploy the built artifact to a repository like Artifactory.

## Dockerfile

```bash
# Dockerfile

FROM openjdk:17-jdk-alpine
WORKDIR /app
COPY .mvn/ .mvn
COPY mvnw pom.xml ./
RUN ./mvnw dependency:resolve
COPY src ./src
CMD ["./mvnw", "spring-boot:run"]
```

## Explanation

```bash
FROM openjdk:17-jdk-alpine
```
1) This line specifies the base image to use for the new image. In this case, it uses the OpenJDK 17 image based on Alpine Linux, which is a lightweight Linux distribution.

```bash
WORKDIR /app
```
2) Sets the working directory inside the container to /app. This is the directory where subsequent commands will be executed.

```bash
COPY .mvn/ .mvn
```
3) Copies the .mvn/ directory from the host (the directory where the Dockerfile is located) to the /app/.mvn/ directory inside the container. This directory likely contains Maven wrapper files.

```
COPY mvnw pom.xml ./
```

4)	Copies the mvnw Maven wrapper and pom.xml file from the host to the /app/ directory inside the container. The pom.xml file contains the project's Maven configuration.

```bash
RUN ./mvnw dependency:resolve
```
5) Executes the Maven wrapper (mvnw) inside the container to resolve project dependencies. This step ensures that all necessary dependencies are downloaded and available for the build process.

```bash
COPY src ./src
```
6) Copies the src/ directory (containing the source code of the application) from the host to the /app/src/ directory inside the container.

```bash
CMD ["./mvnw", "spring-boot:run"]
```
7) Specifies the default command to run when the container starts. In this case, it uses the Maven wrapper (mvnw) to run the Spring Boot application (spring-boot:run), which starts the Spring Boot application inside the container.

## Final Results

1)	Docker
![Screenshot 2024-03-25 213335](https://github.com/logicopslab/spring-petclinic-project/assets/82759985/18246cdb-19c9-49c9-a8c5-fa6b16a31aed)

2)	Jenkins (In Blue Ocean Plugin)
![Screenshot 2024-03-26 225932](https://github.com/logicopslab/spring-petclinic-project/assets/82759985/f8378a90-767b-43e6-b236-f69c9bf3f43b)


3)	SonarQube
![Screenshot 2024-03-26 230152](https://github.com/logicopslab/spring-petclinic-project/assets/82759985/221ac697-2fe7-476e-af6a-25ade4951e27)

