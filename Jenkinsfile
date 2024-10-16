pipeline {
    agent any

    environment {
        registry = "https://registry.hub.docker.com"
        registryCredential = "Docker-hub-Credentials"
        imageName = "latest"
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
                    def app = docker.build("${registry}/${imageName}")
                }
            }
        }
        stage('Push Image To Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        app.push('latest')
                    }
                }
            }
        }
    }
}
