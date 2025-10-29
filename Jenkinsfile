pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = 'github-credentials-id'   // Jenkins GitHub credentials ID
        SFTP_CREDENTIALS = 'sftp-credentials-id'    // Jenkins username/password credentials ID
        SFTP_HOST = 'ac-easapi.jccc.edu'
        REMOTE_PATH = '/home/xzhang24/'
    }

    // For webhook
    triggers {
        githubPush()
    }
    
    stages {
        stage('Checkout from GitHub') {
            steps {
                echo 'Checkout from GitHub ...'
                git branch: 'main', url: 'https://github.com/xzhang24-jccc/jenkins-integration.git'
            }
        }

        triggers {
            githubPush()
        }
        
        stage('SFTP files to test account') {
            steps {
                echo 'SFTP files to test account ...'
                script {
                    withCredentials([usernamePassword(
                        credentialsId: "${SFTP_CREDENTIALS}", 
                        usernameVariable: 'SSH_USER', 
                        passwordVariable: 'SSH_PASS')]) {
                        sh '''
                        # Using sftp batch mode
                        TIMESTAMP=$(date +"%Y%m%d_%H%M%S")     
                        DESTDIR=/home/xzhang24/sftpJenkins_$TIMESTAMP
                        sshpass -p "${SSH_PASS}" sftp -o StrictHostKeyChecking=no xzhang24@ac-easapi.jccc.edu <<EOF
                        mkdir $DESTDIR
                        cd $DESTDIR
                        
                        put -r * $DESTDIR
                        bye
EOF
                        '''
                    }
                }
            }
        }
    }
}
