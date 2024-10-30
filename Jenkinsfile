pipeline {
    agent any

    environment {
       
        RECIPIENTS = 'durgamalleshreddy@gmail.com'
    }

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

        stage('Initialize Terraform') {
            steps {
                dir('path/to/terraform/dir') { // Change to your Terraform directory
                    script {
                        // Initialize Terraform
                        sh 'terraform init'
                    }
                }
            }
        }

        stage('Create EKS Cluster') {
            steps {
                dir('path/to/terraform/dir') { // Change to your Terraform directory
                    script {
                        // Apply Terraform code to create EKS cluster
                        sh 'terraform apply -auto-approve'
                    }
                }
            }
        }

        stage('Deploy Kubernetes Manifests') {
            steps {
                dir('path/to/k8s/dir') { // Change to your Kubernetes directory
                    script {
                        // Configure kubectl to use the newly created EKS cluster
                        sh 'aws eks update-kubeconfig --name <your-eks-cluster-name>'

                        // Deploy your Kubernetes manifests
                        sh 'kubectl apply -f <path-to-your-manifests-directory>'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Successfully created EKS cluster and deployed manifests.'
            emailext(
                subject: "SUCCESS: EKS Cluster Creation and Deployment",
                body: "The pipeline was successful. EKS Cluster has been created and manifests have been deployed.",
                to: "${RECIPIENTS}"
            )
        }
        failure {
            echo 'Failed to create EKS cluster or deploy manifests.'
            emailext(
                subject: "FAILURE: EKS Cluster Creation and Deployment",
                body: "The pipeline has failed. Please check the logs for details.",
                to: "${RECIPIENTS}"
            )
        }
    }
}
