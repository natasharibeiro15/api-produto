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

    }
}