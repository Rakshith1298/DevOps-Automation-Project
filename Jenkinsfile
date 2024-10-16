pipeline {
    agent any

    environment {
        KUBECONFIG = "/var/lib/jenkins/kubeconfig"
        AWS_DEFAULT_REGION = 'ap-south-1' // Update with your desired region
        EKS_CLUSTER_NAME = 'rakshith-kops' // Name of your EKS cluster
        // KUBE_CONFIG = credentials('kubeconfig') // Jenkins credential for kubeconfig file
        DOCKER_IMAGE = 'rakshith98/kops:latest' // Docker image from Docker Hub
        KUBE_NAMESPACE = 'default' // Namespace in EKS cluster
        DEPLOYMENT_NAME = 'kops-demo' // Name of the deployment
        registry = "rakshith98/kops"        // DockerHub repository or username
        registryCredential = "Docker-hub-Credentials"        // Jenkins credential for DockerHub
        imageName = "rakshith-kops"                          // The name for your Docker image
        tag = "latest"                                       // Tag for the Docker image, e.g., 'latest'
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
                    app = docker.build(registry + ":$BUILD_NUMBER")
                }
            }
        }

        stage('Push Image To Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        // Use 'app' (the built image) to push the image
                        app.push("${env.BUILD_NUMBER}")
                        app.push("${tag}")
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-eks-credential']]) {
                    script {
                        sh 'aws eks update-kubeconfig --name rakshith-kops --region ap-south-1'
                        sh 'kubectl apply -f deployment.yml'
                    }
                }
            }
        }
    }
}
