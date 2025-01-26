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
                    sh ' aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 992382545251.dkr.ecr.us-east-1.amazonaws.com '               
                    sh ' docker build -t fredrick-repository . '
                    sh ' docker tag fredrick-repository:latest 992382545251.dkr.ecr.us-east-1.amazonaws.com/fredrick-repository:latest '
                    sh ' docker push 992382545251.dkr.ecr.us-east-1.amazonaws.com/fredrick-repository:latest '
                
                
                
                }
            }
        }
        
    }
}
