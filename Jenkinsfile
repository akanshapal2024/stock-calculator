pipeline {
    agent any
    
    environment {
        AWS_ACCESS_KEY_ID = 'AKIAQIJRRY5HZ2DBTL77'
        AWS_SECRET_ACCESS_KEY = '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'
        AWS_DEFAULT_REGION = 'us-east-1'
        GIT_REPO = 'https://github.com/akanshapal2024/stock-calculator.git'
        DOCKER_IMAGE = 'akanshapal/stock-calculator:latest'
        KUBE_NAMESPACE = 'jenkins' // Kubernetes namespace to deploy to
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Clone the Git repository
                git branch: 'main', credentialsId: 'git-credentials', url: "${GIT_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Run the Docker build command using bat (Windows batch command)
                    bat "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Add your test commands if applicable
                    bat 'echo Running tests...'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Replace 'dockerhub-credentials' with your Jenkins Docker credentials ID
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKERHUB_PASS', usernameVariable: 'DOCKERHUB_USER')]) {
                        // Log in to Docker Hub or your Docker registry
                        bat "docker login -u %DOCKERHUB_USER% -p %DOCKERHUB_PASS%"
                        
                        // Push the Docker image
                        bat "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply Kubernetes Deployment and Service files
                    bat 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            cleanWs() // Clean workspace after the pipeline
        }
    }
}

