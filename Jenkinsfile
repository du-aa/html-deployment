pipeline {
    agent any
    stages {
        stage('Deploy HTML') {
            steps {
                sshagent(['apache-deploy-key']) { // Use the correct SSH credentials ID
                    script {
                        echo 'Deploying index.html to remote Apache server...'
                        
                        // Add the server's SSH key to known hosts
                        sh 'ssh-keyscan -H http://54.87.15.219 >> ~/.ssh/known_hosts'
                        
                        // Copy the file to the remote server using SCP
                        sh 'scp index.html ubuntu@http://54.87.15.219:/var/www/html/'
                    }
                }
            }
        }
        stage('Verify Deployment') {
            steps {
                script {
                    echo 'Verifying Hello.html deployment on remote server...'
                    
                    // Verify the file is served correctly using curl
                    sh 'curl http://54.87.15.219//index.html'
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
