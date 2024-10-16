pipeline {
    agent any

    environment {
        registry = "rakshith98/kops"                      // DockerHub repository or username
        registryCredential = "Docker-hub-Credentials"   // Jenkins credential for DockerHub
        imageName = "rakshith-kops"                     // The name for your Docker image
        tag = "latest"                                  // Tag for the Docker image, e.g., 'latest'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Building Docker Image') {
            steps {
                script {
                    // Build the Docker image and tag it
                    dockerImage = docker.build("${imageName}:${tag}")
                }
            }
        }

        stage('Push Image To Docker Hub') {
            steps {
               script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("${env.BUILD_NUMBER}")
                        dockerImage.push("${tag}")
                    }
                }
            }
        }
    }
}
