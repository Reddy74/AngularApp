pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = "670166063118.dkr.ecr.us-east-1.amazonaws.com/angularapp"
        //ECS_CLUSTER_NAME = "rnr-ecs"
        AWS_REGION = "us-east-1"
    }
    stages {
        stage('Fetch source code') {
            steps {
                git 'https://github.com/Reddy74/AngularApp.git'
            }
        }
        stage('Build Docker image') {
            steps {
                sh 'docker build -t angularapp:latest .'
            }
        }
        stage('Push Docker image to registry') {
            steps {

                    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS    --password-stdin 670166063118.dkr.ecr.us-east-1.amazonaws.com"
                  sh  " docker tag angularapp:latest 670166063118.dkr.ecr.us-east-1.amazonaws.com/angularapp:latest"
                    sh " docker push 670166063118.dkr.ecr.us-east-1.amazonaws.com/angularapp:latest"
                }
            }
        }
    }
}
