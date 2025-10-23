pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = 'github-credentials-id'   // Jenkins GitHub credentials ID
        SFTP_CREDENTIALS = 'sftp-credentials-id'    // Jenkins username/password credentials ID
        SFTP_HOST = 'your.sftp.server.com'
        REMOTE_PATH = '/remote/path'
    }
    
    stages {
        stage('Checkout from GitHub') {
            steps {
                git branch: 'main', url: 'https://github.com/xzhang24-jccc/jenkins-integration.git'
            }
        }

        stage('SFTP files to test account') {
            steps {
                echo 'SFTP files to test account ...'
                script {
                    withCredentials([usernamePassword(
                        credentialsId: "${SFTP_CREDENTIALS}", 
                        usernameVariable: 'SFTP_USER', 
                        passwordVariable: 'SFTP_PASS')]) {
                        sh """
                        # Using sftp batch mode
                        sftp -i xzhang24@ac-easapi.jccc.edu <<EOF
                        put -r * /home/xzhang24/
                        bye
EOF
                        """
                    }
                }
            }
        }
    }
}
