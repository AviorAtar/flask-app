pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                script {
                    sh ' echo " starting to build" '
                    sh ' echo " hio " '
                    sh ' sudo docker build -t simple-flask-app:latest .'
                    sh '  docker run -d -p 5000:5000 simple-flask-app ' 
                }
            }
        }

    stage('test') {
        steps {
            script {
                sh ' echo " test " '
                sh ' curl curl http://127.0.0.1:5000 '
            }
        }
    }

        stage('deploy') {
            steps {
                script {
                    sh ' echo "deploy" '
                    sh ' aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/c0m6s3p2 '               
                    sh ' docker build -t avior_flask-app-repository . '
                    sh ' docker tag avior_flask-app-repository:latest public.ecr.aws/c0m6s3p2/avior_flask-app-repository:latest '
                    sh ' docker push public.ecr.aws/c0m6s3p2/avior_flask-app-repository:latest '
                
                
                
                }
            }
        }
        
    }
}
