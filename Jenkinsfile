pipeline {
    agent any

    environment {
        // Map your 'Secret text' credential ID to an environment variable
        K8S_TOKEN  = credentials('k8s-token')
        EKS_SERVER = 'https://F6E0DB8A4BBFCD3A2F9EAF3FDA8CF269.gr7.ap-southeast-2.eks.amazonaws.com'
    }

    stages {
        stage('Deploy To Kubernetes') {
            steps {
                script {
                    // We pass the token, server URL, and namespace directly into the CLI
                    sh """
                        kubectl apply -f deployment-service.yml \
                          --server=${EKS_SERVER} \
                          --token=\$K8S_TOKEN \
                          --namespace=webapps \
                          --insecure-skip-tls-verify
                    """
                }
            }
        }
        
        stage('Verify Deployment') {
            steps {
                script {
                    sh """
                        kubectl get svc \
                          --server=${EKS_SERVER} \
                          --token=\$K8S_TOKEN \
                          --namespace=webapps \
                          --insecure-skip-tls-verify
                    """
                }
            }
        }
    }
}
