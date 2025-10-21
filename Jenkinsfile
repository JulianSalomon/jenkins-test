pipeline {
	agent any
	/* agent {
		docker {
			image 'maven:3.8.1'
		}
	} */

	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		javaHome = tool 'jdk8'
		PATH = "$PATH:${dockerHome}/bin:${mavenHome}/bin:${javaHome}/bin"
	}

	stages {
		stage('Checkout') {
			steps {
				sh 'docker --version'
				sh 'mvn --version'

				echo "PATH: $PATH"
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