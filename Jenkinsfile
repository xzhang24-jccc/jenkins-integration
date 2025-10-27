pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = 'github-credentials-id'   // Jenkins GitHub credentials ID
        SFTP_CREDENTIALS = 'sftp-credentials-id'    // Jenkins username/password credentials ID
        SFTP_HOST = 'ac-easapi.jccc.edu'
        REMOTE_PATH = '/home/xzhang24/'
    }
    
    stages {
        stage('Checkout from GitHub') {
            steps {
                echo 'Checkout from GitHub ...'
                git branch: 'main', url: 'https://github.com/xzhang24-jccc/jenkins-integration.git'
            }
        }

        stage('SFTP files to test account') {
            steps {
                echo 'SFTP files to test account ...'
                script {
                    withCredentials([usernamePassword(
                        credentialsId: "${SFTP_CREDENTIALS}", 
                        usernameVariable: 'SSH_USER', 
                        passwordVariable: 'SSH_PASS')]) {
                        sh """
                        
                        sshpass -p "${SSH_PASS}" sftp -o StrictHostKeyChecking=no xzhang24@ac-easapi.jccc.edu <<EOF
                                                # Using sftp batch mode
                        TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
                
                        # Create directory with timestamp
                        mkdir -p sftpFromJenkins_$TIMESTAMP
                
                        # Change into that directory
                        cd sftpFromJenkins_$TIMESTAMP
                        
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
