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

        stage('Update Current Deployment') {
            steps {
                script {
                    // Stop the existing container
                    sh 'docker stop pb-simple-app-container || true' // Stop the container if it exists
                    
                    // Remove the existing container
                    sh 'docker rm pb-simple-app-container || true' // Remove the container if it exists
                    
                    // Pull the updated image from Docker Hub
                    sh 'docker pull aujung/pb-simple-app:latest'
                    
                    // Run the new container with the updated image
                    sh 'docker run -d --name pb-simple-app-container -p 80:3000 aujung/pb-simple-app:latest'
                }
            }
        }
    }
}
