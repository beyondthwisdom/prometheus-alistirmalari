#!/usr/bin/env groovy

pipeline {
    agent any
    environment {
        ECR_REPO_URL = '664574038682.dkr.ecr.eu-west-3.amazonaws.com'
        IMAGE_REPO = "${ECR_REPO_URL}/java-app"
        IMAGE_NAME = "1.0-${BUILD_NUMBER}"
        CLUSTER_NAME = "my-cluster"
        CLUSTER_REGION = "eu-west-3"
    }
    stages {
        stage('build app') {
            steps {
               script {
                   echo "building the application..."
                   sh './gradlew clean build'
               }
            }
        }
        stage('build image') {
            steps {
                script {
                    echo "building the docker image..."
                    sh "sudo docker build -t ${IMAGE_REPO}:${IMAGE_NAME} ."
                    sh "aws ecr get-login-password --region ${CLUSTER_REGION} | sudo docker login --username AWS --password-stdin ${ECR_REPO_URL}"
                    sh "sudo docker push ${IMAGE_REPO}:${IMAGE_NAME}"

                }
            }
        }
        stage('deploy') {
            environment {
                AWS_ACCESS_KEY_ID = credentials('jenkins_aws_access_key_id')
                AWS_SECRET_ACCESS_KEY = credentials('jenkins_aws_secret_access_key')
                APP_NAME = 'java-app'
                APP_NAMESPACE = 'my-app'
            }
            steps {
                script {
                    echo 'deploying new release to EKS...'
                    
                }
            }
        }
    }
}