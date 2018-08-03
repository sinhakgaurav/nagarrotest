pipeline {
    agent any
	
    stages {
        
        stage('Maven is Compiling Application') {
            steps{
                withMaven(maven :'maven'){
                    echo 'compiling application step'
                    bat 'mvn clean compile'
                }
            }

        } 
        stage('Junit is Performing Test') {
            steps {
                withMaven(maven :'maven'){
                    echo 'scripts to run tests...'
                    bat 'mvn test'
                }    
            }
        }
        stage('Sonar is Analysing Code') {
            steps {
                withMaven(maven :'maven'){
                    echo 'Running code check....'
                    bat 'mvn sonar:sonar'
                }
            }
        }
        stage ('Deploying Artifacts'){
            steps{
				withMaven(maven :'maven'){
                    echo 'Running code check....'
                    bat 'mvn deploy'
                }
                
            }
        }
    }
}

