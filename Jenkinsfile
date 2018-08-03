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
				script {
					def server = Artifactory.server 'default'
					def buildInfo = Artifactory.newBuildInfo()
					buildInfo.env.capture = true
					buildInfo.env.collect()
					def rtMaven = Artifactory.newMavenBuild()
					rtMaven.tool = 'Maven3'
					rtMaven.deployer releaseRepo:'Maven-repo-Pipeline', snapshotRepo:'Maven-repo-Pipeline', server: server
					rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo
					buildInfo.retention maxBuilds: 10, maxDays: 7, deleteBuildArtifacts: true 
				  // Publish build info.
					server.publishBuildInfo buildInfo
				}
                
            }
        }
    }
}

