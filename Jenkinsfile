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
            echo "BUILD DONE"
            // image = docker.build("${registry}:v${BUILD_NUMBER}")  
          }
        }
        }
   
     stage('Push image') {
      steps{
          script{
             docker.withRegistry('', dockerhubCreds )
            def image = docker.image("${registry}")
            image.push()
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
