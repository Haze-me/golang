pipeline {
    agent any

    environment {
       docker_ID = credentials('docker_ID') // Jenkins credential ID
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
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_ID') {
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
