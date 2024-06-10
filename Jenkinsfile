pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    def customImage = docker.build("aujung/pb-simple-app")
                }
            }
        }
        stage('Push To Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        def customImage = docker.image("aujung/pb-simple-app")
                        customImage.push("latest")
                    }
                }
            }
        }
        stage('Deploy to Kubernetes with Kubectl') {
            steps {
                script {
                    withKubeCredentials(kubeconfigWrite: true) {
                        sh "kubectl apply -f deployment.yaml"
                    }
                }
            }
        }
    }
}