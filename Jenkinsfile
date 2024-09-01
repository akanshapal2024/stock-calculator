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
                    sh 'echo Building the application...'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh 'echo Running tests...'
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

