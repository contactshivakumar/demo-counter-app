pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/contactshivakumar/demo-counter-app.git'
            }
        }

         stage('Unit test') {

             steps {
                bat 'mvn test'
             }
          }

           stage('Integration test') {

             steps {
                bat 'mvn verify -DskipUnitTests'
           }

           }
          stage('Maven Build') {

             steps {
                bat 'mvn clean install'
           }

           }

           stage('Static Code Analysis') {

             steps {
                withSonarQubeEnv(installationName:'sonarserver', credentialsId: 'sonar-api') {
                bat 'mvn clean package sonar:sonar'
             }
           }
        }

           stage('Quality Gate Status') {

             steps {
             timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                }
           }
        }

           stage('Upload to nexus') {

             steps {
             script {
                def pomVersion = readMavenPom file: 'pom.xml'
                def nexusRepo = pomVersion.version.endsWith("SNAPSHOT")?"demoapp-snapshot":"demoapp-release"
                nexusArtifactUploader artifacts: [
                [artifactId: 'springboot',
                classifier: '',
                file: 'target/Uber.jar', type: 'jar']
                ],
                credentialsId: 'nexus-auth1',
                groupId: 'com.example',
                nexusUrl: 'localhost:8081',
                nexusVersion: 'nexus3',
                protocol: 'http',
                repository: nexusRepo,
                version: "${pomVersion.version}"
                }
           }
        }

    }
}