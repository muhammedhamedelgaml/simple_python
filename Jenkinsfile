pipeline {
    agent any
        environment {

      registry = "muhammedhamedelgaml/app_python"
      dockerhubCreds = credentials('dockerhub')
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
            //  sh ' echo $dockerhubCreds_PSW | docker login -u $dockerhubCreds_USR --password-stdin '
             withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            sh  '   docker login --username $USERNAME --password $PASSWORD  '
            //  docker.withRegistry('', dockerhubCreds )

            // def image = docker.image("${registry}")
            // image.push()
          }
        }
    }


    post {
        always {
            echo 'Pipeline finished!'
        }
    }
}
