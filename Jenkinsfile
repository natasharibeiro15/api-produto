pipeline{

    agent any

    stages{
        stage('Build Image'){
            steps{
                 script {
                    dockerapp = docker.build("natasharibeiro15/api-produto:${env.BUILD_ID}", '-f ./src/Dockerfile ./src') 
                } 
            }
        }

        stage ('Push Image') {
            steps {
                script {
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
            if (isUnix()) {
                // Execute comandos Kubernetes no ambiente Unix-like (Linux, macOS)
                sh 'sed -i "s/{{tag}}/$tag_version/g" ./k8s/deployment.yaml'
                sh 'kubectl apply -f ./k8s/deployment.yaml'
            } else {
                // Execute comandos Kubernetes no ambiente Windows (PowerShell)
                powershell "(Get-Content ./k8s/deployment.yaml) | ForEach-Object { $_ -replace '{{tag}}', '$tag_version' } | Set-Content ./k8s/deployment.yaml"
                powershell "kubectl apply -f .\\k8s\\deployment.yaml"
            }
        }
    }
}

    }
    }