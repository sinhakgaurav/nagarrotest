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
                dir("C:\Users\kumarsinha\devops"){
                    script {
                        def server = Artifactory.server('artifactory')
                        def rtMaven = Artifactory.newMavenBuild()
                        rtMaven.resolver server: server, releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot'
                        rtMaven.deployer server: server, releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local'
                        rtMaven.tool = 'maven 3'
                        def buildInfo = rtMaven.run pom: 'pom.xml', goals: 'install'
                        server.publishBuildInfo buildInfo
                    }
                }
            }
        }
    }
}

