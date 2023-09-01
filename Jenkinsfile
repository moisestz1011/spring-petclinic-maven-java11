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
				withSonarQubeEnv ('SonarQubeToken'){
			sh "mvn sonar:sonar \
					-Dsonar.projectKey=spring-petclinic-maven-java11 \
				-Dsonar.host.url=http://44.211.159.89:9000"
				}
			}
		}
		stage('Wait for SonarQube'){
			steps{
				timeout(time:3, unit: 'MINUTES'){
					script{
						waitForQualityGate abortPipeline:true
					}
				}
			}
		}
	}
}
