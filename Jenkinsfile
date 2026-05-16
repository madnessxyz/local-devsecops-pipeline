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
                // Code analysis commands will go here in the next step
            }
        }
    }
}
