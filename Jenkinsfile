pipeline {
	agent none

	options {
		timeout(time: 5, unit: 'MINUTES')
		// buildDiscarder(logRotator(numToKeepStr: '10'))
	}

	environment {
		DEBUG = 'true'
		DOCKER_IMAGE = "juliansalomon/currency-exchange-microservice"
	}

	tools {
		dockerTool 'myDocker'
	}

	stages {
		stage('Build and Test') {
			agent {
				docker {
					image 'maven:3.9.11-eclipse-temurin-8'
					args '-v $HOME/.m2:/root/.m2 -u 1000:1000'
					reuseNode true
				}
			}
			stages {
				stage('Checkout') {
					when {
						environment name: 'DEBUG', value: 'true'
					}
					steps {
						sh 'java -version'
						sh 'mvn --version'

						echo "PATH: $PATH"
						echo "HOME: $HOME"
						echo "BUILD_NUMBER: ${env.BUILD_NUMBER}"
						echo "BUILD_ID: ${env.BUILD_ID}"
						echo "BUILD_TAG: ${env.BUILD_TAG}"
						echo "BUILD_URL: ${env.BUILD_URL}"
						echo "WORKSPACE: ${env.WORKSPACE}"
						echo "JOB_NAME: ${env.JOB_NAME}"
					}
				}
				stage('Build') {
					steps {
						echo 'Building...'
						sh 'mvn clean compile'
					}
				}
				stage('Tests') {
					parallel {
						stage('Test') {
							steps {
								echo 'Testing...'
								sh 'mvn test'
							}
						}
						stage('Integration Test') {
							steps {
								echo 'Running Integration Tests...'
								sh "mvn failsafe:integration-test failsafe:verify"
							}
						}
					}
					post {
						always {
							junit '**/target/surefire-reports/*.xml, **/target/failsafe-reports/*.xml'
						}
					}
				}
				stage('Package') {
					steps {
						sh 'mvn package -DskipTests=true'
						stash includes: 'target/*.jar', name: 'app-jar'
					}
				}
				stage('Archive') {
					steps {
						archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
					}
				}
			}
		}

		stage('Docker Operations') {
			agent any
			stages {
				stage('Docker Build') {
					steps {
						unstash 'app-jar'
						script {
							dockerImage = docker.build("${env.DOCKER_IMAGE}:${env.BUILD_TAG}")
						}
					}
				}

				stage('Docker Push') {
					steps {
						script {
							docker.withRegistry('', 'dockerhub') {
								dockerImage.push()
								dockerImage.push('latest')
							}
						}
					}
				}
			}
		}
	}
	
	post {
		always {
			echo 'This will always run after the stages.'
		}
		success {
			echo 'This will run only if the pipeline succeeds.'
		}
		failure {
			echo 'This will run only if the pipeline fails.'
		}
		changed {
			echo 'This will run if there are changes in the codebase.'
		}
		unstable {
			echo 'This will run if the build is unstable, e.g., tests fail.'
		}
	}
}
