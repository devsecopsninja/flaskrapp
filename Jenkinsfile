pipeline {
  environment {
    registry = "devsecopsninja/devsecops"
    registryCredential = "dockerhub"
    dockerImage = ''
 
  }
  
  agent any
  
  stages {
        stage('Build Docker Image') {
      steps {
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    
    stage('Push to DockerHub') {
      steps {
        script {
          docker.withRegistry('', registryCredential ) {
            dockerImage.push('latest')
          }
        }
      }
    }
    
    stage('Deploy Test Application') {
      steps {
        sh 'docker stop flaskr && docker rm flaskr || true'
        sh 'docker pull devsecopsninja/devsecops:latest'
        sh 'docker run -d -p 5000:5000 --name flaskr devsecopsninja/devsecops:latest'
      }
    }
       
   } 
}
