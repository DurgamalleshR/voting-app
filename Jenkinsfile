pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from your Git repository
                checkout scm
            }
        }

        stage('Login to AWS') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'Aws-credentials']]) {
                    script {
                        // Verify AWS credentials
                        sh 'aws sts get-caller-identity'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Successfully logged in to AWS.'
        }
        failure {
            echo 'Failed to log in to AWS.'
        }
    }
}
