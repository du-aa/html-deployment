pipeline {
    agent any
    environment {
        APACHE_HOST = 'your-apache-server-ip'         // Replace with the Apache server's IP
        APACHE_USER = 'ec2-user'                     // Replace with the Apache server's user
    }
    stages {
        stage('Checkout') {
            steps {
                // Pull the latest code from GitHub
                git branch: 'main', url: 'https://github.com/your-username/html-deployment.git'
            }
        }
        stage('Deploy to Apache') {
            steps {
                // SSH into the Apache server and deploy the file
                sshagent(['apache-ssh-credentials']) {  // Use the Jenkins credential ID for the SSH key
                    sh """
                    scp index.html ${APACHE_USER}@${APACHE_HOST}:/var/www/html/
                    ssh ${APACHE_USER}@${APACHE_HOST} 'sudo systemctl restart httpd'
                    """
                }
            }
        }
    }
}
