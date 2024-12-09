pipeline {
    agent any
        environment {

      registry = "muhammedhamedelgaml/app_python"
      dockerhubCreds = 'dockerhub'
    }
    stages {
        stage('Build image') {
        steps{
          script{
            image = docker.build( registry + "v:$BUILD_NUMBER" )     
          }
        }
        }
   
     stage('Push image') {
      steps{
          script{
             docker.withRegistry(dockerhubCreds)
             image.push("$BUILD_NUMBER")
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
