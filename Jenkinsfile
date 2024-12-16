pipeline {
    agent any
    environment {
        image_name = "muhammedhamedelgaml/app_python"
    }
    stages {
        stage('Build image') {
            steps {
                script {
                    echo "BUILD DONE"
                    dockerimage = docker.build("${image_name}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {   
                    sh '''
                        docker login --username $USERNAME --password $PASSWORD
                    '''
                    dockerimage.push()
                }
            }
        }

        stage("Ansible Deploy to vagrant VMs") {
            steps {
                script {
                    ansiblePlaybook(
                        inventory     : 'ansible/inventory',
                        playbook      : 'ansible/site.yml',
                        installation  : 'ansible',  
                        colorized     : false,
                        extraVars     : [
                            IMAGE: "${image_name}",
                            TAG: "${BUILD_NUMBER}"
                        ]
                    )
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished! \n logging out from docker'
            sh 'docker logout'
        }
    }
}
