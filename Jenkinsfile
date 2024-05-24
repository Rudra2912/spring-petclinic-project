/*pipeline {
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
*/

///New Jenkins File Copy and Paste in the jenkins groovy pipeline to execute


pipeline {
    agent any
    tools{
        jdk 'jdk17'
        maven 'maven3'
    }
    environment {
         DOCKERHUB_CREDENTIALS = credentials('rudra-doc')
       //  DOCKERHUB_USERNAME = 'Rudresh29'
        // DOCKERHUB_PASSWORD = 'Satoro@29'
    }
    stages {
        stage ('Clean'){
            steps{
                cleanWs()    
            }
            
        }
        stage('Git Code Checkout') {
            steps {
            git branch: 'main', credentialsId: 'rudra-doc', url: 'https://github.com/Rudra2912/spring-petclinic-project.git'}
        }
        stage('Clean and Install') {
            steps {
                sh 'mvn clean install '
            }
        }
        stage('Build the application') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }
        //stage ('Code Quality on server'){
        //    steps {
        //        withSonarQubeEnv('sonar'){
        //           sh ''' mvn clean verify sonar:sonar -Dsonar.projectName=Rudra_Spring_proj \
        //           -Dsonar.java.binaries=./target/classes \
        //           -Dsonar.projectKey=Rudra_Spring_Proj-generator \
        //           -Dsonar.organization=rudra2912 '''
        //       }
        //     }
        //}
        stage ('Code Quality on cloud'){
            steps {
                withSonarQubeEnv('rudra sonarcloud'){
                   sh ''' mvn clean verify sonar:sonar -Dsonar.projectName=Rudra_Spring_proj \
                   -Dsonar.java.binaries=./target/classes \
                   -Dsonar.projectKey=Rudra_Spring_Proj-generator \
                   -Dsonar.organization=rudra2912 '''
               }
             }
        }
        stage('Docker Build') { 
            steps {
                sh 'docker build -t rudresh29/petclinic:Latest .'
                sh ' docker logout'
            }
        }
        //stage('Docker Image Vulnerability Scan') { 
        //     steps {
        //         // this would be done by using SNYK
        //        snykSecurity additionalArguments: '--docker rudresh29/petclinic', failOnIssues: false, organisation: 'rudra2912', projectName: 'rudra spring', snykInstallation: 'synk', snykTokenId: 'rudra-synk-main', targetFile: 'Dockerfile'
        //    }
        //}
       
        stage ('DockerHub Push'){
            steps {
                script{
                    withCredentials([usernamePassword(credentialsId: 'rudra-doc', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                        sh 'docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}'
                        sh "docker push rudresh29/petclinic:latest"
                    }
                }
                
            }
        }
        stage ('DockerHub Pull and Deploy'){
            steps {
                script{
                    withCredentials([usernamePassword(credentialsId: 'rudra-doc', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                        sh 'docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}'
                        sh "docker container rm -f rudresh29_petclinic"
                        sh "docker container run -d --name rudresh29_petclinic -p 12223:8080 rudresh29/petclinic:latest"
                    }
                }
                
            }
        }
        
    }
}
