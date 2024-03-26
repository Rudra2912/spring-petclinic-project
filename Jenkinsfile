pipeline {
    agent any
    environment {
            MAVEN_HOME = 'C:/Installers/apache-maven-3.9.6'
            PATH = "C:\\WINDOWS\\SYSTEM32;${MAVEN_HOME}\\bin;${PATH}"
            DOCKERHUB_CREDENTIALS = credentials('ravishrawat-dh')
        }
    stages {
        stage('Clean and Install') {
            steps {
                bat 'mvn clean install'
            }
        }
        stage('Build the application') {
            steps {
                bat 'mvn package'
            }
        }
        stage('Run Tests') {
            steps {
                bat 'mvn test'
            }
        }
        stage ('Code Quality'){
            steps {
                withSonarQubeEnv('SonarQube Server') {
                bat 'mvn sonar:sonar'
                }
             }
        }
        stage('Docker Build') { 
            steps {
                bat 'docker build -t ravishrawat/petclinic:latest .'
            }
        }
    }
}