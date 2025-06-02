pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
               sh 'exho "Executando o comando Docker Build""'     
            }
        } 
        stage('Push Docker Image') {
            steps {
               sh 'exho "Executando o comando Docker Push""'     
            }
        } 
        stage('Deploy no Kubernetes') {
            steps {
               sh 'exho "Executando o comando kubectl apply""'     
            }
        } 
    }
}