pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code is ready'
            }
        }

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

        stage('Build Complete') {
            steps {
                echo 'Jenkins Pipeline completed successfully!'
            }
        }
    }
}
