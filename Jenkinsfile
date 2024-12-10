pipeline {
    agent any
        environment {

      registry = "muhammedhamedelgaml/app_python"
      // dockerhubCreds = 'dockerhub'
    }
    stages {
        stage('Build image') {
        steps{
          script{
            echo "BUILD DONE"
            // image = docker.build("${registry}:v${BUILD_NUMBER}")  
          }
        }
        }
   
     stage('Push image') {
      steps{
          script{
  withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            sh """
                   docker login --username $USERNAME --password $PASSWORD 
                   docker push muhammedhamedelgaml/simple_python:v13
               """
          }
        }
     }

    }


    post {
        always {
            echo 'Pipeline finished!'
        }
    }
}
