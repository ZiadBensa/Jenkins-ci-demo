pipeline {
    agent any

    environment {
        IMAGE_NAME = "jenkins-ci-lab"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    echo "🔧 Building Docker image..."
                    def app = docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo "🧪 Running tests..."
                    docker.image("${IMAGE_NAME}:latest").inside {
                        sh 'npm test'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Deploying (simulated)..."
            }
        }
    }

    post {
        always {
            echo "📋 Pipeline completed at: ${new Date()}"
        }
    }
}
