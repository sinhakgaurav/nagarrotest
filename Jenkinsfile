pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                bat 'mvn clean install;'
                bat "echo 'shell scripts to build project...';"
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

