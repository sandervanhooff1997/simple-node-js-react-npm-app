pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000'
        }
    }
    environment { 
        CI = 'true'
    }
    stages {
        stage('SonarQube analysis') {
            // requires SonarQube Scanner 2.8+
            def scannerHome = tool 'SonarQube Scanner 2.8';
            withSonarQubeEnv('SonarQube Scanner') {
            sh "${scannerHome}/bin/sonar-scanner"
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
               
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}