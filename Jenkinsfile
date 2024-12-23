pipeline {
    agent any

    stages {
        stage('Deploy HTML') {
            steps {
                sshagent(['apache']) {
                    script {
                        echo 'Deploying index.html to remote Apache server...'
                        
                        // Add the server's SSH key to known_hosts for the correct IP
                        sh 'ssh-keyscan -H 3.83.119.254 >> ~/.ssh/known_hosts'

                        // Copy the file to the remote server using the correct IP
                        sh 'scp index.html ubuntu@3.83.119.254:/var/www/html/'
                    }
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    echo 'Verifying index.html deployment on remote server...'
                    // Verify the file is served correctly using the correct IP
                    sh 'curl http://3.83.119.254/index.html'
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
