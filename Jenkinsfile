pipeline {
    agent any

    stages {

        stage('Check Node.js') {
            steps {
                sh 'node --version'
                sh 'npm --version'
            }
        }

        stage('Test Application') {
            steps {
                sh 'node --check app.js'
            }
        }

        stage('Deploy to System 2') {
            steps {
                sh '''
                    rsync -avz \
                    -e "ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/.ssh/jenkins_deploy_key" \
                    --exclude='.git' \
                    ./ ubuntu@172.31.45.198:/home/ubuntu/remote-app/
                '''
            }
        }

        stage('Install Dependencies and Restart App') {
            steps {
                sh '''
                    ssh -o StrictHostKeyChecking=no \
                    -i /var/lib/jenkins/.ssh/jenkins_deploy_key \
                    ubuntu@172.31.45.198 '
                        cd /home/ubuntu/remote-app &&
                        npm install &&
                        pm2 restart app || pm2 start app.js --name app
                    '
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    ssh -o StrictHostKeyChecking=no \
                    -i /var/lib/jenkins/.ssh/jenkins_deploy_key \
                    ubuntu@172.31.45.198 '
                        pm2 status
                    '
                '''
            }
        }
    }
}
