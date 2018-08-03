pipeline {
    agent any
	
    stages {
        
        stage('Build') {
            steps{
                withMaven(maven :'maven'){
                    echo 'build step'
                    bat 'mvn clean compile'
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
        }
        stage('Code Check') {
            steps {
                withMaven(maven :'maven'){
                    echo 'Running code check....'
                    bat 'mvn sonar:sonar'
                }
            }
        }
        stage ('Artifactory Deploy'){
            steps{
				withMaven(maven :'maven'){
                    echo 'Running code check....'
                    bat 'mvn deploy'
                }
                
            }
        }
    }
}

