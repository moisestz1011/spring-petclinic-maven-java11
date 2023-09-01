pipeline {
	agent any
	tools{
		maven "Maven 3.8.6"
	}
	
	stages{
 		stage('Build artifact'){
			steps{
			sh "mvn clean package -DskipTests=true"
			archive 'target/*.jar'
			}
		}
		stage('Test Maven - JUnit'){
			steps{
			sh "mvn test"
			}
			post{
				always{
					junit 'target/surefire-reports/*.xml'
				}
			}
		}
		stage('Sonarqube Analysis - SAST'){
			steps{
				withSonarQubeEnv ('SonarQube'){
			sh "mvn sonar:sonar \
					-Dsonar.projectKey=NewJenkinsProject \
				-Dsonar.host.url=http://54.161.181.11:9000"
				}
			}
		}
		stage('Wait for SonarQube'){
			steps{
				timeout(time:2, unit: 'MINUTES'){
					script{
						waitForQualityGate abortPipeline:true
					}
				}
			}
		}
	}
}
