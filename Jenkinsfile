pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("aujung/pb-simple-app")
                }
            }
        }

        stage('Push To docker hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        docker.image("aujung/pb-simple-app").push("latest")
                    }
                }
            }
        }

        stage('Update current deployment') {
            steps {
                script {
                    sh 'docker stop pb-simple-app-container || true'
                    
                    sh 'docker rm pb-simple-app-container || true'
                    
                    sh 'docker pull aujung/pb-simple-app:latest'
                    
                    sh 'docker run -d --name pb-simple-app-container -p 3000:3000 aujung/pb-simple-app:latest'
                }
            }
        }
    }
}
