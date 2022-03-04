pipeline {
  agent {
    kubernetes {
     yamlFile 'k8s/jenkins-pod.yaml'
    }
  }
  environment {
        REPO = 'azureacr1dzenancin.azurecr.io'
    }

  stages {
    stage('Build') {
      steps {
       container('node'){// no container directive is needed as the maven container is the default
       sh "npm install"
        sh "ng build --prod --output-path=./build"
        }
      }
    }
    stage('Build Docker Image') {
      steps {
        container('docker') {
          sh "/kaniko/executor --context `pwd` --destination ${REPO}/app:1.0"
        }
      }
    }
  }
}
