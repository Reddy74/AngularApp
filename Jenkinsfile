pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = "670166063118.dkr.ecr.us-east-1.amazonaws.com/angularapp"
        ECS_CLUSTER_NAME = "rnr-ecs"
        AWS_REGION = "ap-south-2"
    }
    stages {
        stage('Fetch source code') {
            steps {
                git 'https://github.com/Reddy74/AngularApp.git'
            }
        }
        stage('Build Maven project') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Build Docker image') {
            steps {
                sh 'docker build -t srirepo:latest .'
            }
        }
        stage('Push Docker image to registry') {
            steps {

                    sh "aws ecr get-login-password --region ap-south-2 | docker login --username AWS    --password-stdin 670166063118.dkr.ecr.ap-south-2.amazonaws.com"
                  sh  " docker tag srirepo:latest 670166063118.dkr.ecr.ap-south-2.amazonaws.com/srirepo:latest"
                    sh " docker push 670166063118.dkr.ecr.ap-south-2.amazonaws.com/srirepo:latest"
                }
            }
        
        stage('Deploy to EKS cluster') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'sri-eks', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                    sh "aws eks update-kubeconfig --region ap-south-2 --name sri-eks"
                    sh 'kubectl get svc '
                    sh 'kubectl apply -f adok8.yaml'
                }
            }
        }
    }
}
