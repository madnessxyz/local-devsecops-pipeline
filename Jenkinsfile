pipeline {
    agent { label 'agent' } // Instructs the Master to run this entire pipeline on our isolated runner node!

    environment {
        SCANNER_HOME = tool 'sonar-scanner' // Maps our automated SonarQube quality analysis engine
        HOST_IP = '192.168.1.173'
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
                withSonarQubeEnv('sonar-server') {
                    sh "${SCANNER_HOME}/bin/sonar-scanner \
                    -Dsonar.projectKey=Wanderlust-3Tier \
                    -Dsonar.projectName=Wanderlust-3Tier \
                    -Dsonar.sources=."
                }
            }
        }

        stage('🐳 Build Container Images') {
            steps {
                echo 'Compiling multi-tier application using hardened optimized Dockerfiles...'
                sh 'docker build -t wanderlust-backend:latest -f ./backend/Dockerfile_optimized ./backend'
                sh 'docker build -t wanderlust-frontend:latest -f ./frontend/Dockerfile_optimized ./frontend'
            }
        }

        stage('🏴‍☠️ Trivy Image Vulnerability Scan') {
            steps {
                echo 'Unleashing Trivy container vulnerability sweep...'
                sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v $HOME/.cache:/root/.cache aquasec/trivy:latest image --severity CRITICAL wanderlust-backend:latest'
                sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v $HOME/.cache:/root/.cache aquasec/trivy:latest image --severity CRITICAL wanderlust-frontend:latest'
            }
        }

        stage('🚀 Local Deployment') {
            steps {
                echo 'Deploying verified application stack directly via Host Docker Engine...'
                // Using Jenkins credentials provider to safely SSH onto the host and run modern compose commands
                sshagent(credentials: ['build-agent']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no jenkins@${HOST_IP} '
                            cd ~/DevOps-Projects/DevOps-Project-39/wanderlust-3tier-project && \
                            docker compose down && \
                            docker compose up -d --build
                        '
                    """
                }
                echo 'Application is live and running on production host ports!'
            }
        }
    }
}
