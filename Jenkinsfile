pipeline {
    agent any
    environment {
        registry = "muhammedhamedelgaml/app_python"
    }
    stages {
        stage('Build image') {
            steps {
                script {
                    echo "BUILD DONE"
                    // docker.build("${registry}:v${BUILD_NUMBER}")
                }
            }
        }
   
        stage('Push image') {
            steps {
            withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {   
                sh '''
                       docker login --username $USERNAME --password $PASSWORD     
                       docker push $registry           
                    '''
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline finished! \n logout from docker'
   
            sh ' docker logout  '
        }
    }
}
