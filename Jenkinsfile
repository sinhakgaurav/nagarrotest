pipeline {
    agent any

    stages {
        stage('Git checkout') {
            steps {
                git credentialsId: 'b1179787-95ad-44b1-b76a-8046e12b268e', url: 'https://github.com/sinhakgaurav/nagarrotest.git'
            }
        }
        stage('Build') {
            steps{
                withMaven(maven :'LocalMAVEN'){
                    sh 'mvn -f pom.xml clean compile'
                }
            }

        } 
        stage('Test') {
            steps {
                sh 'mvn test;'
                sh "echo 'shell scripts to run static tests...';"
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh '/jenkins/scripts/deliver.sh;'
                sh "echo 'Deploying the application now...';"
            }
        }
    }
}

