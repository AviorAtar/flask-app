pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        ECR_REPO_URI = 'public.ecr.aws/c0m6s3p2/avior_flask-app-repository'
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("flask-app:${IMAGE_TAG}")
                }
            }
        }

        stage('Push to AWS ECR') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'aws-ecr-credentials', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                        sh "aws ecr create-repository --repository-name flask-app-repository --region ${AWS_REGION} || true"
                        sh "docker tag flask-app:${IMAGE_TAG} ${ECR_REPO_URI}:${IMAGE_TAG}"
                        sh "docker push ${ECR_REPO_URI}:${IMAGE_TAG}"
                    }
                }
            }
        }

        stage('Deploy on EC2') {
            steps {
                script {
                    sshagent(['ec2-ssh-key']) {
                        sh "docker pull ${ECR_REPO_URI}:${IMAGE_TAG}"
                        sh "docker run -d -p 5000:5000 ${ECR_REPO_URI}:${IMAGE_TAG}"
                    }
                }
            }
        }
    }
}
