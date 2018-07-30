pipeline {
    agent any

    stages {
        
        stage('Build') {
            steps{
                withMaven(maven :'maven'){
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

