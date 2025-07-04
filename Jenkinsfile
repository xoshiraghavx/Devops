pipeline {
    agent any
    environment {
        IMAGE_NAME = "fastapi-hruday"
        CONTAINER_NAME = "fastapi_container"
        APP_PORT = "8000"
    }
    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'github-creds',
                    branch: 'main', 
                    url: 'https://github.com/xoshiraghavx/Devops.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }
        stage('Stop & Remove Old Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''
            }
        }
        stage('Run Docker Container') {
            steps {
                sh '''
                docker run -d --name $CONTAINER_NAME -p $APP_PORT:$APP_PORT $IMAGE_NAME
                '''
            }
        }
    }
    post {
        success {
            echo "✅ FastAPI app deployed successfully at port $APP_PORT"
        }
        failure {
            echo "❌ Deployment failed. Check Jenkins logs."
        }
    }
}
