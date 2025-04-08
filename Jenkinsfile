pipeline {
    agent any
  stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: 'd40aa1b6-2a91-43f1-929d-536bd7bb1190', url: 'https://github.com/Neerajtank94/myrepo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    
                    sh "docker build -t 738649861708.dkr.ecr.us-east-1.amazonaws.com/getkart/myrepo:${env.BUILD_ID} ."
                    
                }
            }
        }

        stage('Push Image to ECR') {
            steps {
                script {
                    
                    sh "docker push 738649861708.dkr.ecr.us-east-1.amazonaws.com/getkart/myrepo:${env.BUILD_ID}"
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                script {
                    sh "aws eks update-kubeconfig --region us-east-1 --name ${EKS_CLUSTER_NAME}"
                    sh "kubectl apply -f k8s/deployment.yaml"
                    sh "kubectl apply -f k8s/service.yaml"
                }
            }
        }
    }
}
