pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script {
          docker.build('feedback-app')
        }
      }
    }
    stage('Deploy') {
      steps {
        kubernetesDeploy(
          kubeconfigId: 'kube-config-id',
          configs: 'k8s/deployment.yml'
        )
      }
    }
    stage('Version Control Check out') {
        steps {
            checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Rakshith1298/DevOps-Automation-Project']])
        }
      }
    }
}
