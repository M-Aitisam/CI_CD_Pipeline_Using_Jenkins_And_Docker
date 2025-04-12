pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "yourdockerhubusername/my-app"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/yourusername/your-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh "docker rm -f my-app-container || true"
                sh "docker run -d -p 5000:5000 --name my-app-container ${DOCKER_IMAGE}:latest"
            }
        }
    }
}
