pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-credentials') // Add your Docker Hub credentials in Jenkins
    }
    stages {
        stage('Checkout SCM') {
            steps {
                git 'https://github.com/Rakshith1298/DevOps-Automation-Project.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('rakshith98/rakshith-kops-project')
                }
            }
        }
        stage('Push Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        docker.image('rakshith98/rakshith-kops-project').push('latest')
                    }
                }
            }
        }
    }
}
