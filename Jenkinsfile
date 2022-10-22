pipeline {
    agent any
environment {
    PATH = "D:\\Softwares\\Git\\usr\\bin;C:\\Softwares\\Git\\bin;${env.PATH}"
    }
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/contactshivakumar/demo-counter-app.git'
            }
        }

         stage('Unit test') {
             steps {
                sh 'mvn test'
             }
          }

    }
}