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

        stage('JFrog') {
            steps {
                    withCredentials([usernamePassword(credentialsId: 'JFrog_Artifactory_Credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        rtServer (
                            id: "Artifactory",
                            url: 'http://localhost:8082/artifactory',
                            username: USERNAME,
                            password: PASSWORD,
                            bypassProxy: true,
                            timeout: 300
                        )
                    }
                }
            }
        stage('Artifact Upload'){
            steps{
                    rtUpload (
                     serverId:"Artifactory" ,
                      spec: '''{
                       "files": [
                          {
                          "pattern": "*.jar",
                          "target": "petclinic"
                          }
                                ]
                               }''',
                            )
                }
            }
        stage ('Publish build info') {
            steps {
                    rtPublishBuildInfo (
                        serverId: "Artifactory"
                    )
                }
            }        

        stage('Docker Build') { 
            steps {
                bat 'docker build -t ravishrawat/petclinic:latest .'
            }
        }
        // stage('Docker Image Vulnerability Scan') { 
        //     steps {
        //         // this would be done by using SNYK
        //         snykSecurity additionalArguments: '--docker ravishrawat/petclinic', snykInstallation: 'Snyk', snykTokenId: 'Snyk-Jenkins', targetFile: 'Dockerfile'
        //     }
        // }
        stage ('DockerHub Push'){
            steps {
                echo "Docker Login and Push Placeholder"
            }
        }
    }
}
