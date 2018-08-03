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
                dir("devops"){
                    script {
                        def server = Artifactory.server('default')
                        def rtMaven = Artifactory.newMavenBuild()
                        rtMaven.resolver server: server, releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot'
                        rtMaven.deployer server: server, releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local'
                        rtMaven.tool = 'maven'
                        def buildInfo = rtMaven.run pom: 'pom.xml', goals: 'install'
                        server.publishBuildInfo buildInfo
                    }
                }
            }
        }
    }
}

