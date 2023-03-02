pipeline{
    agent any
    	environment {
		notifyEmail ="ravish.bagasi@nagarro.com"
	}
    tools{
        maven 'Maven'
    }
    stages{
        stage("code checkout"){
            steps{
            bat "echo hello"
            }
        }   
        stage("code build"){
            steps{
            bat "mvn clean"
            }
        }
        stage("unit test"){
            steps{
            bat "mvn test"
            }
        }
        stage("Sonar Analysis"){
            steps{
            withSonarQubeEnv("Test_Sonar")
                {
		    bat "echo Sonar Run half"
                        bat "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.8.0.2131:sonar"        
                }
            }
        }
        stage("Publish to Artifactory"){
            steps{
                rtMavenDeployer(
                    id: 'deployer',
                    serverId: 'Ravish_Artifactory',
                    releaseRepo: 'Ravish_Artifactory',
                    snapshotRepo: 'Ravish_Artifactory'
                )
                rtMavenRun(
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: 'deployer'
                    )
                rtPublishBuildInfo(
                    serverId:'Ravish_Artifactory',
                )
            }        
        }
        stage("Invoke UI Test Pipeline"){
			steps{
				build job: 'Dev-Ops-Freestyle-Practice'
			}
		}
    }
    post{
        success{
            bat "echo success"
            }
        }
}
