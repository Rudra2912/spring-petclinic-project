Project Documentation - PetClinic

Setup Guide

Pre requisites

1)	Git (https://git-scm.com/)
2)	Java 17 (https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html)
3)	Maven 3.9.6 (https://maven.apache.org/download.cgi)
4)	Jenkins (https://www.jenkins.io/download)
5)	Sonar installed (https://www.sonarsource.com/products/sonarqube/downloads/
Version – Community. SetUp on - http://localhost:9000/
Start from C:\Installers\sonarqube-10.4.1.88267\bin\windows-x86-64
6)	Docker installed ()
7)	Code IDE – VS Code
   
Steps to run the project

There are 2 ways in which this project can be up and running

1)	Local setup
2)	Using Jenkins
   
Method 1 – Local setup

1)	Clone the repo in your local machine
2)	Go inside spring-petclinic-project folder
3)	Open a command prompt with the same location and run mvn package
4)	Once successful it will produce a jar file
 

8)	Run it by java -jar spring-petclinic-3.2.0-SNAPSHOT.jar
It will be up and running in some time

Method 2 – Using Jenkins
1)	Install Jenkins, do the basic setup, once it is up and running.
2)	Create a new pipeline, use Jenkinsfile

https://github.com/logicopslab/spring-petclinic-project/blob/main/Jenkinsfile

3)	Install Docker desktop
4)	Install SonarQube, do the basic setup. Run it on unused port.
Once it is up and running, kick off the build in Jenkins.
5)	Make sure the build is passed.
6)	Once the project runs with SonarQube, it will show these results

Jenkinsfile explanation

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

Dockerfile explanation

1)	FROM openjdk:17-jdk-alpine: This line specifies the base image to use for the new image. In this case, it uses the OpenJDK 17 image based on Alpine Linux, which is a lightweight Linux distribution.
2)	WORKDIR /app: Sets the working directory inside the container to /app. This is the directory where subsequent commands will be executed.
3)	COPY .mvn/ .mvn: Copies the .mvn/ directory from the host (the directory where the Dockerfile is located) to the /app/.mvn/ directory inside the container. This directory likely contains Maven wrapper files.
4)	COPY mvnw pom.xml ./: Copies the mvnw Maven wrapper and pom.xml file from the host to the /app/ directory inside the container. The pom.xml file contains the project's Maven configuration.
5)	RUN ./mvnw dependency:resolve: Executes the Maven wrapper (mvnw) inside the container to resolve project dependencies. This step ensures that all necessary dependencies are downloaded and available for the build process.
6)	COPY src ./src: Copies the src/ directory (containing the source code of the application) from the host to the /app/src/ directory inside the container.
7)	CMD ["./mvnw", "spring-boot:run"]: Specifies the default command to run when the container starts. In this case, it uses the Maven wrapper (mvnw) to run the Spring Boot application (spring-boot:run), which starts the Spring Boot application inside the container.

Final Results

1)	 Docker
2)	Jenkins
3)	SonarQube
