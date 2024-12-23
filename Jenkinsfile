pipeline {
    agent any

    stages {
        stage('Deploy HTML') {
            steps {
                sshagent(['apache-deploy-key']) {
                    script {
                        echo 'Deploying Main.html to remote Apache server...'
                        
                        // Add the server's SSH key to known_hosts for the correct IP
                        sh 'ssh-keyscan -H 13.49.72.39 >> ~/.ssh/known_hosts'

                        // Copy the file to the remote server using the correct IP
                        sh 'scp Main.html ubuntu@13.49.72.39:/var/www/html/'
                    }
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    echo 'Verifying Main.html deployment on remote server...'
                    // Verify the file is served correctly using the correct IP
                    sh 'curl http://13.49.72.39/Main.html'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully!'
        }
        failure {
            echo 'Deployment failed. Check the logs for more details.'
        }
    }
}
