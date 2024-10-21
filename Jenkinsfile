pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker_ID') // Jenkins credential ID
        DOCKER_IMAGE = "haze21/go-app"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Haze-me/golang.git'
            }
        }

        stage('Build Go Application') {
            steps {
                sh 'go mod tidy'
                sh 'go build -o app .'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'DOCKER_HUB_CREDENTIALS') {
                        docker.image("${DOCKER_IMAGE}:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
