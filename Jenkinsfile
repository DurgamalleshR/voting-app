pipeline {
    agent any
  environment {
        ARM_CLIENT_ID       = "7e9b470a-90f2-4ed5-9876-7545df39ebbb"
        ARM_CLIENT_SECRET   = "fiw8Q~WaJdB0Jzj3Oyss3hBFX54.9HtJ6K1eYcyj"
        ARM_SUBSCRIPTION_ID = "1f2450e7-ac4d-4c20-b942-4cc2893a3e03"
        ARM_TENANT_ID       = "d5fc86d5-b2e4-49f8-93c0-a9e4d09e5499"
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/DurgamalleshR/voting-app/tree/master/aks', branch: 'main'
            }
        }
        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }
        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -out=tfplan'
            }
        }
        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -input=false tfplan'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'terraform.tfstate', allowEmptyArchive: true
        }
        failure {
            mail to: 'team@example.com',
                subject: "Terraform Apply Failed",
                body: "Please check the Jenkins job."
        }
    }
}
