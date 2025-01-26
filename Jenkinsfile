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
                    sshagent([-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAliEybS3e3X6cqRqvLSgmc0H8mqMagJfk8/IsOre3v53og4HF
ysXpM9Ns4uPCPQA+PxC1w57UFGavvSGJHaY44wKoIW/Km4KfzRlfc7WpIazQtgwg
D5DUX1z7IKlHwQv+Dk0qlRtpSKfOw1GxH0mwED9+9YQg3Q+JRBPgfr2hRp7EV/JI
WkeNPTYq5AYQtVanxGso14RTMHvEvyS0y66CeIUA3z22TQ7fLRri4YVjqMAFiMD9
0TZK3g3p6K7CwACf1ocYs++SagQerJaw14fbTr8x/b1wTvPvqB8Ojq6hbqdx1OOn
CGJXe84M1XcRsnj76wxiWFIqzkvADwd1EUUeJwIDAQABAoIBACnJv8GIHRrGHVEs
rkoBfQJkAa4BbGUS4pXMeOc3MBxUFd5VBvs0L39PgGV/RBe6s5jWS+HRmfK7ynr4
lLP+CqOVn7r+5QR4TYbHYzxP+m33Lnkd0ubCR3rr5LLoq6Jedi8A57NtV9bzbqgL
WyP8XNeI9Qvd5U/i/TBH4BsnKXcAWT8CcpwC92ScOKvjvujYgQFVwxR/0uI7ZHaS
aiqx5mXpK0fP3dpSRlJXni6r5hlyIyd1rmlYQzSNrcQf81JKb9k/KwJSE8o/c7CU
cGhwABhcVG2kEJL4NwN/4bkAu1Jb4gZBi4xtic9TqNejAD8gixejsQuVabRTLiz2
iSZNL2ECgYEAxQMaCmzwn1lD162lfmTLA0RXfOxu82uqAGZ1oXAwZ8PUTHXguzID
JExuEpLk9oHZRC/WTK0wCIeW5/oj4szE8tEeSNOqOaUa1FMJ7xMtm48ao9dbcuIG
VNCcT/apShgRFpqmx/QkEb+srFu5oEPNHm4JLsUZywTUqHvzZkRubmkCgYEAwxST
SVDfF1LJ+YInDQQDxZf6r56WhPwp8azfc/LitaOMHdiMh//1k3Z4UiQ3VRRfsB6q
cruwiNSywgaZlvjn8yMeQyBx8Z43p5aYDdg34y2hvMxAnXpG6iKpbo/nMUrBEbCT
H9SLM3N43BCaVTHsTd3OD5M96dMbnUi/Jzwetg8CgYEAsKw2ZxEIu62eRETpey5C
TDy2REX2vX5YZqF7bXmhPvyDXsn67YRNAvkzYJ6wi0sonphTklFhGNUXBBLyqYm+
OI5FnUOLjMQ+ChgpeWb2aGDtn+4ZiS2eReMJuj/V0qdAc8feNsP+sJf17l8XWNxU
jqxYucNxfPlpB2qYfKZzbbkCgYBtDWd4qLjNtP4reNKTuBbTsq301taV6NEZxOX1
T2Xk3FRLqu6EaFXD5YpFgPFdolILhlkKYlnBZAW56S6a8vHXPV6Bb/XCeAvsp4bA
1ooHeU7c1+GXJu/VmyLUJ6W6LGvZk7frJ0nZrUd23eEolwveS4wRsOH/gIADYNNa
/4S7iQKBgQCucSgm+ZW6IRNPyV0wgcifdaAP4wfycRav4mN5JF4TePAcEJ3cVXHL
bEHtgmtT4MKDgoGZG2VW5r8vlmSzCBp9AtWyc7lAhUBYT60enS/J7enk34oXAULa
XdVOjbPqfzdR0RdZcB6gTTsOOCiN7Tz+R5AMx/yqUaIghMr6xyPH8A==
-----END RSA PRIVATE KEY-----]) {
                        sh "docker pull ${ECR_REPO_URI}:${IMAGE_TAG}"
                        sh "docker run -d -p 5000:5000 ${ECR_REPO_URI}:${IMAGE_TAG}"
                    }
                }
            }
        }
    }
}
