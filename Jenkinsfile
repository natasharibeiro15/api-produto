pipeline {
    agent any

    stages {
        stage('Build Image') {
            steps {
                script {
                    // Construir a imagem Docker
                    dockerapp = docker.build("natasharibeiro15/api-produto:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    // Fazer o push da imagem para o Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    }
}
