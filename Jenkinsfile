pipeline {
    agent { label 'agent' } // Instructs the Master to run this entire pipeline on our isolated runner!

    environment {
        SCANNER_HOME = tool 'sonar-scanner' // Maps our automated SonarQube quality analysis engine
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
                // Build backend and frontend images using their respective optimized multi-stage files
                sh 'docker build -t wanderlust-backend:latest -f ./backend/Dockerfile_optimized ./backend'
                sh 'docker build -t wanderlust-frontend:latest -f ./frontend/Dockerfile_optimized ./frontend'
            }
        }

        stage('🏴‍☠️ Trivy Image Vulnerability Scan') {
            steps {
                echo 'Unleashing Trivy container vulnerability sweep...'
                // Run Trivy via Docker container to scan our freshly built local images
                sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v $HOME/.cache:/root/.cache aquasec/trivy:latest image --severity CRITICAL wanderlust-backend:latest'
                sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v $HOME/.cache:/root/.cache aquasec/trivy:latest image --severity CRITICAL wanderlust-frontend:latest'
            }
        }
    }
}
