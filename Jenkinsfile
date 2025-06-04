pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
               script {
                    dockerapp = docker.build("thiagoresende/guia-jenkins:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
               }     
            }
        } 
        stage('Push Docker Image') {
            steps {
               script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest') 
                        dockerapp.push("${env.BUILD_ID}")    
                    }
               }   
            }
        } 
        stage('Deploy no Kubernetes') {
            steps {
                sh 'echo "Aplicando manifestos Kubernetes ao cluster Minikube..."'
                sh 'kubectl apply -f k8s/deployment.yaml' // Este comando aplica o seu arquivo
                sh 'echo "Deploy no Kubernetes (tentativa de apply) concluído!"'
            }
        } 
    }
}
