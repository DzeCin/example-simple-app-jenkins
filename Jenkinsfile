pipeline {
  agent {
    kubernetes {
     yamlFile 'k8s/jenkins-pod.yaml'
    }
  }
  environment {

        REPO = 'azureacr1dzenancin.azurecr.io'
        AZURE_CLIENT_ID = credentials('jenkins-azure-client-id')
        AZURE_CLIENT_SECRET = credentials('jenkins-azure-client-secret')
        AZURE_TENANT_ID = credentials('jenkins-azure-tenant-id')
    }

  stages {

    stage('Build') {
      steps {
       container('node'){// no container directive is needed as the maven container is the default
        sh "npm install"
        sh "ng build --prod --output-path=./build"
        sh "ls"
        }
      }
    }


    stage('Build Docker Image') {
      steps {
        container('docker') {
          sh "ls"
          sh "/kaniko/executor --context `pwd` --destination ${REPO}/app:1.0"
        }
      }
    }

    stage('Kubectl test') {
      steps {
        container('kubectl') {
          withKubeConfig([credentialsId: 'deployment-sa', serverUrl: 'https://test-0140002d.hcp.westeurope.azmk8s.io:443']) {
             sh 'kubectl get pods -A'
          }
        }
      }
    }

  }
}
