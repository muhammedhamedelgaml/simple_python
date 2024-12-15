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
                    '''
                }
            }
        }

        stage("Ansible Deploy to vagrant VMs") {
            steps {
                script {
                    ansiblePlaybook(
                        inventory: 'ansible/inventory',  // Path to your inventory file
                        playbook: 'ansible/playbook.yml',  // Path to your playbook
                        installation: 'ansible',  // Ensure Ansible is installed in your Jenkins environment
                        colorized: false,  // Set to true if you want color output in the logs
                        credentialsId: 'vm01',  // Credentials ID in Jenkins (contains SSH private key and username)
                        disableHostKeyChecking: true  // Disables the StrictHostKeyChecking to avoid SSH prompts
                    )
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished! \n logout from docker'
            sh 'docker logout'
        }
    }
}
