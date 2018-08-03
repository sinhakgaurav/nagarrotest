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
					//Artifactory server instance declaration   
					def server = Artifactory.server 'default' //server1 is the Server ID given to Artifactory server in Jenkins

					//Capturing the Build artifacts from Build server to upload
					def uploadSpec = """{
						"files": [
							{
								"pattern": "/Users/kumarsinha/.jenkins/workspace/forkgettingready/target/*.jar",
								"target": "devops/"
							}
						]
					}"""
					server.upload(uploadSpec)

					//Capturing the artifacts from Artifactory server to download it into Jenkins machine at /var/lib/jenkins/workspace/JF/jarFile/
					def downloadSpec = """{
						"files": [
							{
								"pattern": "forkgettingready/DevopsTask-1.2-SNAPSHOT.jar",
								"target": "jarFile/" 
							}
						]
					}"""
					server.download(downloadSpec)


					//Publishing build info to Artifactory - Method 2
					def buildInfo = Artifactory.newBuildInfo()
					server.upload spec: uploadSpec, buildInfo: buildInfo
					//server.download spec: downloadSpec, buildInfo: buildInfo
					server.publishBuildInfo buildInfo

					//Capturing Environment variables
					buildInfo.env.capture = true
					buildInfo.env.collect()
					buildInfo.retention maxBuilds: 10, maxDays: 7, deleteBuildArtifacts: true

					// Build Promotion Section
					def promotionConfig = [
						// Mandatory parameters
						'buildName'          : buildInfo.name,
						'buildNumber'        : buildInfo.number,
						'targetRepo'         : 'libs-release-local',

						// Optional parameters
						'comment'            : 'deploy SUCCESSFULLY COMPLETED',
						'sourceRepo'         : 'devops',
						'status'             : 'Released',
						'includeDependencies': false,
						'copy'               : false,
						// 'failFast' is true by default.
						// Set it to false, if you don't want the promotion to abort upon receiving the first error.
						'failFast'           : true
					]

					// Promote build
					//server.promote promotionConfig //this promotes the build automatically to the target specified in promotionConfig 
					Artifactory.addInteractivePromotion server: server, promotionConfig: promotionConfig, displayName: "Promotions Time" //this need human interaction to promote
					echo "Build Completed Successfully and Promotions are manual"
				}
                
            }
        }
    }
}

