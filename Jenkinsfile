pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"
        ECR_REPO = "930797750287.dkr.ecr.ap-south-1.amazonaws.com/nodeapp"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-creds',
                    url: 'https://github.com/sejalrbhor2404/node-jenkins-app.git'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t nodeapp .'
                sh 'docker tag nodeapp:latest $ECR_REPO:latest'
            }
        }

        stage('Push to ECR') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds'
                ]]) {
                    sh '''
                      aws ecr get-login-password --region $AWS_REGION \
                      | docker login --username AWS --password-stdin 930797750287.dkr.ecr.ap-south-1.amazonaws.com

                      docker push $ECR_REPO:latest
                    '''
                }
            }
        }
    }
}
