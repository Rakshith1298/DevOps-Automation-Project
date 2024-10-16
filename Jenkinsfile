pipeline {
    agent any

    environment {
        registry = "rakshith98/rakshith-kops-project"                      // DockerHub repository or username
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
                    app = docker.build("${imageName}:${tag}")
                }
            }
        }

        stage('Push Image To Docker Hub') {
            steps {
                script {
                    // Push the image to Docker Hub with credentials
                    docker.withRegistry('https://hub.docker.com/repository/docker/rakshith98/rakshith-kops-project', registryCredential) {
                        app.push("${tag}")   // Push the Docker image with the specified tag (e.g., 'latest')
                    }
                }
            }
        }
    }
}
