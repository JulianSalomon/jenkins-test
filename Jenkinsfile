pipeline {
	agent {
		docker {
			image 'maven:3.6.3'
		}
	}

	stages {
		stage('Build') {
			steps {
				sh 'mvn --version'
				echo 'Building...'
				sh 'mvn clean install -DskipTests'
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
		changed {
			echo 'This will run if there are changes in the codebase.'
		}
		unstable {
			echo 'This will run if the build is unstable, e.g., tests fail.'
		}
	}
}