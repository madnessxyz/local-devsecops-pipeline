pipeline {
    agent { label 'agent' } // Instructs the Master to run this entire pipeline on our isolated runner!

    environment {
        SCANNER_HOME = tool 'sonar-scanner' // This will map our automated security analysis engine later
    }

    stages {
        stage('🧹 Clean Workspace') {
            steps {
                cleanWs()
                echo 'Workspace cleared, preparing for code analysis...'
            }
        }

        stage('📦 Git Checkout') {
            steps {
                checkout scm
                echo 'Code successfully synchronized from GitHub repository.'
            }
        }

	        stage('🛡️ SonarQube Code Quality Scan') {
            steps {
                echo 'Initiating deep SecOps source code vulnerabilities scan...'
		withSonarQubeENV('sonar-server') {
	            sh "${SCANNER_HOME}/bin/sonar-scanner \
                    -Dsonar.projectKey=Wanderlust-3Tier \
                    -Dsonar.projectName=Wanderlust-3Tier \
                    -Dsonar.sources=."
            }
        }
    }
}
