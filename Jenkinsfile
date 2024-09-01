pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the repository
                git 'https://github.com/akanshapal2024/stock-calculator.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                script {
                    docker.build('stock-calculator')
                }
            }
        }

        stage('Run Tests') {
            steps {
                // Run any tests if you have
                echo 'Running tests...'
                // Add your test scripts here
            }
        }

        stage('Deploy') {
            steps {
                // Run the Docker container
                script {
                    sh 'docker run -d -p 5000:5000 stock-calculator'
                }
            }
        }
    }
}

