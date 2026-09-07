pipeline {
    agent any

    parameters {
        password(
            name: 'AWS_ACCESS_KEY_ID',
            defaultValue: '',
            description: 'AWS Access Key ID'
        )

        password(
            name: 'AWS_SECRET_ACCESS_KEY',
            defaultValue: '',
            description: 'AWS Secret Access Key'
        )
    }

    environment {
        AWS_REGION = 'ap-south-1'
        AWS_ACCOUNT_ID = '378494867940'
        ECR_REGISTRY = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
        IMAGE_TAG = "1.0.${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('ECR Login') {
            steps {
                withEnv([
                    "AWS_ACCESS_KEY_ID=${params.AWS_ACCESS_KEY_ID}",
                    "AWS_SECRET_ACCESS_KEY=${params.AWS_SECRET_ACCESS_KEY}"
                ]) {
                    sh '''
                    aws ecr get-login-password --region $AWS_REGION | \
                    docker login --username AWS \
                    --password-stdin $ECR_REGISTRY
                    '''
                }
            }
        }

        stage('Build Images') {
            steps {
                sh '''
                docker build -t streaming-auth:$IMAGE_TAG backend/authService

                docker build -t streaming-stream:$IMAGE_TAG \
                -f backend/streamingService/Dockerfile backend

                docker build -t streaming-admin:$IMAGE_TAG \
                -f backend/adminService/Dockerfile backend

                docker build -t streaming-chat:$IMAGE_TAG \
                -f backend/chatService/Dockerfile backend

                docker build -t streaming-frontend:$IMAGE_TAG frontend
                '''
            }
        }

        stage('Tag and Push') {
            steps {
                sh '''
                for svc in auth stream admin chat frontend
                do
                    docker tag streaming-$svc:$IMAGE_TAG \
                    $ECR_REGISTRY/streaming-$svc:$IMAGE_TAG

                    docker push \
                    $ECR_REGISTRY/streaming-$svc:$IMAGE_TAG
                done
                '''
            }
        }
    }

    post {
        success {
            echo 'StreamingApp build and ECR push successful'
        }

        failure {
            echo 'StreamingApp pipeline failed'
        }
    }
}
