pipeline {
    agent any
        environment {

      registry : "muhammedhamedelgaml/app_python"
      dockerhubCreds : 'dockerhub'
    }
    stages {
        stage('Build image') {
          script{
            image = docker.build( registry + "v:$BUILD_NUMBER" )     
          }
        }
   
     stage('Push image') {
          script{
             docker.withRegistry(dockerhubCreds)
             image.push("$BUILD_NUMBER")
          }
        }
   
   
    }


    post {
        always {
            echo 'Pipeline finished!'
        }
    }
}
