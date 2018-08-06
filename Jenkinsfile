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
				echo "deploying artifacts and capturing build information"
				script {
					def server = Artifactory.server 'default' 
					
					def rtMaven = Artifactory.newMavenBuild()
					rtMaven.tool = 'maven'
					rtMaven.opts = '-Xms1024m -Xmx4096m'
					
					
					rtMaven.resolver server: server, releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot'
					rtMaven.deployer server: server, releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local'
					def buildInfo1 = rtMaven.run pom: 'pom.xml', goals: 'clean install'
					
					def uploadSpec = """{
						"files": [
							{
								"pattern": "/workspace/forkgettingready/target/*.jar",
								"target": "devops/"
							}
						]
					}"""
					server.upload(uploadSpec)

					//Capturing the artifacts from Artifactory server to download it into Jenkins machine at /var/lib/jenkins/workspace/JF/jarFile/
					def downloadSpec = """{
						"files": [
							{
								"pattern": "com/mycompany/DevopsTask/3.0-SNAPSHOT/DevopsTask-3.0-SNAPSHOT.pom",
								"target": "jarFile/" 
							}
						]
					}"""
					buildInfo1.append server.download(downloadSpec)


					//Publishing build info to Artifactory - Method 2
					def buildInfo = Artifactory.newBuildInfo()
					 
					//Capturing Environment variables
					buildInfo.env.capture = true
					buildInfo.env.collect()
					
					buildInfo.append buildInfo1
					
					buildInfo.retention maxBuilds: 10, maxDays: 7, deleteBuildArtifacts: true
					server.upload spec: uploadSpec, buildInfo: buildInfo
					server.download spec: downloadSpec, buildInfo: buildInfo
					server.publishBuildInfo buildInfo


					echo "Build Completed Successfully and Promotions are manual"
				}
                
            }
        }
		
		stage('Build Image') {
			steps {
			    sh 'docker build -t dockerim .'
				}
			}
		stage('Deploy to Dockerhub') {
			steps {
				script {
					withDockerRegistry([credentialsId: 'dockerhub', url: '']) {
						sh 'docker tag dockerim kumarsinha/devops:dockerim'  
						sh 'docker push kumarsinha/devops:dockerim'
					}
				}
			}
		}
	}
}


