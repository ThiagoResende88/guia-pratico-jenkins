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
                // 'minikube-kubeconfig' é o ID da credencial que você criou no Jenkins
                // KUBECONFIG_FILE será uma variável de ambiente contendo o caminho para o arquivo kubeconfig
                withCredentials([file(credentialsId: 'minikube-kubeconfig', variable: 'KUBECONFIG_FILE')]) {
                   sh 'echo "Aplicando manifestos Kubernetes ao cluster Minikube..."'
                   // Diz ao kubectl para usar este arquivo de configuração específico
                   sh "KUBECONFIG=${KUBECONFIG_FILE} kubectl apply -f k8s/deployment.yaml"
                   sh 'echo "Deploy no Kubernetes (tentativa de apply) concluído!"'
                   }
             }
        }
     }
}
