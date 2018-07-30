pipeline {
    agent any

    stages {
        
        stage('Build') {
            steps{
                withMaven(maven :'maven'){
                    echo 'build step'
                    bat 'mvn clean install'
                }
            }

        } 
        stage('Test') {
            steps {
                withMaven(maven :'maven'){
                    echo 'scripts to run tests...'
                    bat 'mvn test'
                    
            }
        }
        stage('Code Check') {
            steps {
                withMaven(maven :'maven'){
                    echo 'Running code check....'
                    bat 'mvn sonar:sonar'
                    
            }
        }
        stage('Deliver') {
            steps {
                echo 'Deploying the application now...'
            }
        }
    }
}

