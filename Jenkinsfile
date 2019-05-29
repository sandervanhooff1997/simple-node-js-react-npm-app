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
        stage('Sonarqube') {
            tools {
                jdk "jdk-1.8.101"
            }
            environment {
                scannerHome = tool 'SonarQubeScanner'
            }
            steps {
                sh 'java -version'
                withSonarQubeEnv('SonarCubeServer') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
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