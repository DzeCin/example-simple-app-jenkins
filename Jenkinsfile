pipeline {
  agent {
    kubernetes {
     yamlFile 'k8s/jenkins-pod.yaml'
    }
  }
  stages {
    stage('Build') {
      steps {
       container('node'){// no container directive is needed as the maven container is the default
        sh "ng build code --prod --output-path=./build"
        }
      }
    }
    stage('Build Docker Image') {
      steps {
        container('docker') {
          sh "docker build -t azureacr1dzenancin.azurecr.io/app:dev ."  // when we run docker in this step, we're running it via a shell on the docker build-pod container,
          sh "docker push azureacr1dzenancin.azurecr.io/app:dev"        // which is just connecting to the host docker deaemon
        }
      }
    }
  }
}
