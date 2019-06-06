pipeline {
    agent none
    environment { 
        CI = 'true'
    }
    stages {
        // stage('Sonarqube') {
        //     agent { 
        //         docker 'openjdk:8-jre' 
        //     }
        //     environment {
        //         scannerHome = tool 'SonarQubeScanner'
        //     }
        //     steps {
        //         sh 'java -version'
        //         withSonarQubeEnv('sonarqube') {
        //             sh "${scannerHome}/bin/sonar-scanner -X"
        //         }
        //         timeout(time: 10, unit: 'MINUTES') {
        //             waitForQualityGate abortPipeline: true
        //         }
        //     }
        // }
        stage('Build') {
            agent {
                docker {
                    image 'node:6-alpine'
                }
            }
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'node:6-alpine'
                }
            }
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }

        stage('Deliver for development') {
            agent {
                docker {
                    image 'node:6-alpine'
                    args '-p 3000:3000'
                }
            }
            when {
                branch 'development'
            }
            steps {
                sh './jenkins/scripts/deliver-for-development.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }

        stage('Deploy for production') {
            agent {
                docker {
                    image 'node:6-alpine'
                    args '-p 5000:5000'
                }
            }
            when {
                branch 'production'  
            }
            steps {
                sh './jenkins/scripts/deploy-for-production.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }

        // old deliver stage
        // stage('Deliver') { 
        //     agent {
        //         docker {
        //             image 'node:6-alpine'
        //             args '-p 3000:3000'
        //         }
        //     }
        //     steps {
        //         sh './jenkins/scripts/deliver.sh' 
        //         sh './jenkins/scripts/kill.sh' 
        //     }
        // }
    }
}