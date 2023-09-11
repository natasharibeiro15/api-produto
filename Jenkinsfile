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

        sstage('Deploy Kubernetes') {
    environment {
        tag_version = "${env.BUILD_ID}"
    }
    steps {
        script {
            def tag = env.tag_version
            bat "Get-Content ./k8s/deployment.yaml | ForEach-Object { \$_ -replace '{{tag}}', '${tag}' } | Set-Content -Path ./k8s/deployment.yaml -Force"
            bat 'kubectl apply -f ./k8s/deployment.yaml'
        }
    }
}

    }
}
