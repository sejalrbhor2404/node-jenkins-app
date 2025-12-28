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
            aws --version

            aws ecr get-login-password --region ap-south-1 \
            | docker login --username AWS --password-stdin 930797750287.dkr.ecr.ap-south-1.amazonaws.com

            docker tag nodeapp:latest 930797750287.dkr.ecr.ap-south-1.amazonaws.com/nodeapp:latest
            docker push 930797750287.dkr.ecr.ap-south-1.amazonaws.com/nodeapp:latest
            '''
        }
    }
}

                }
            }
        }
    }
}
