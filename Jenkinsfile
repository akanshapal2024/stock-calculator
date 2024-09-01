pipeline {
    agent any
    
    environment {
        GIT_REPO = 'https://github.com/akanshapal2024/stock-calculator.git'
        DOCKER_IMAGE = 'akanshapal/stock-calculator:latest'
        KUBE_NAMESPACE = 'default' // Kubernetes namespace to deploy to
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Clone the Git repository
                git branch: 'main', credentialsId: 'git-credentials', url: "${GIT_REPO}"
            }
        }

        stage('Build Application') {
            steps {
                script {
                    // Example build step for Windows - Adjust according to your application
                    bat 'echo Building the application...'
                    // Add your build commands here, e.g., `npm install`, `mvn package`, etc.
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Example test step for Windows
                    bat 'echo Running tests...'
                    // Add your test commands here, e.g., `npm test`
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    dockerImage = docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to the registry
                    docker.withRegistry('', 'dockerhub-credentials') {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply Kubernetes Deployment and Service files
                    sh 'kubectl apply -f deployment.yaml'
                    sh 'kubectl apply -f service.yaml'
                }
            }
        }
    }

    post {
        always {
            cleanWs() // Clean workspace after pipeline execution
        }
    }
}

