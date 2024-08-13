pipeline {
    agent any
    environment {
        REGION = "ap-southeast-1"        
        FRONTEND_APP = "frontend"
        BACKEND_APP = "backend"
        ECR_URI = "533267130345.dkr.ecr.ap-southeast-1.amazonaws.com" // change
        IMAGE_TAG = "${BUILD_NUMBER}"
        GIT_REPO = "https://github.com/thuylien87/backend.git"
        REPO_NAME = "backend"
    }

    

    stages {
        stage('Docker Build Backend') {
            agent any
            steps {
                withAWS(region:'ap-southeast-1',credentials:'aws-credential') {
                    sh "aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ECR_URI}"
                    sh "docker build -t ${BACKEND_APP}:${IMAGE_TAG} src/backend/"
                    sh "docker tag ${BACKEND_APP}:${IMAGE_TAG} ${ECR_URI}/${BACKEND_APP}:latest"
                    sh "docker push ${ECR_URI}/${BACKEND_APP}:latest"
                }
            }
        }

        stage('Docker Build Frontend') {
            agent any
            steps {
                withAWS(region:'ap-southeast-1',credentials:'aws-credential') {
                    sh "aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ECR_URI}"
                    sh "docker build -t ${FRONTEND_APP}:${IMAGE_TAG} src/frontend/"
                    sh "docker tag ${FRONTEND_APP}:${IMAGE_TAG} ${ECR_URI}/${FRONTEND_APP}:latest"
                    sh "docker push ${ECR_URI}/${FRONTEND_APP}:latest"
                }
            }
        }
    }
}
