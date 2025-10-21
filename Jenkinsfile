pipeline {
	agent any

	stages {
		stage('Build') {
			steps {
				echo 'Building...'
				// Add build steps here
			}
		}
		stage('Test') {
			steps {
				echo 'Testing...'
				// Add test steps here
			}
		}
		stage('Integration Test') {
			steps {
				echo 'Running Integration Tests...'
				// Add deploy steps here
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
	}
}