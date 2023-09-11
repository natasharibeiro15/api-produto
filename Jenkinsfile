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

        stage('Deploy Kubernetes') {
            environment {
                tag_version = "${env.BUILD_ID}"
            }
            steps {
                script {
                    // Substituir a tag no arquivo de configuração do Kubernetes
                    bat "powershell -Command (Get-Content ./k8s/deployment.yaml -Raw) -replace '{{tag}}', '$env:tag_version' | Set-Content ./k8s/deployment.yaml"
                    
                    // Aplicar as configurações no cluster Kubernetes (assumindo que o kubectl está configurado no PATH)
                    bat "kubectl apply -f ./k8s/deployment.yaml"
                }
            }
        }
    }
}
