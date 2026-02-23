pipeline {
    agent any

    environment {
        DOCKER_CREDS = credentials('docker-creds')
        BACKEND_IMAGE = "sanikakate2002/backendapp:1.1"
        FRONTEND_IMAGE = "sanikakate2002/frontendapp:1.0"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/yourusername/student-app.git'
            }
        }

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t $BACKEND_IMAGE:${BUILD_NUMBER} ./backend'
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh 'docker build -t $FRONTEND_IMAGE:${BUILD_NUMBER} ./frontend'
            }
        }

        stage('Login to DockerHub') {
            steps {
                sh 'echo $DOCKER_CREDS_PSW | docker login -u $DOCKER_CREDS_USR --password-stdin'
            }
        }

        stage('Push Images') {
            steps {
                sh 'docker push $BACKEND_IMAGE:${BUILD_NUMBER}'
                sh 'docker push $FRONTEND_IMAGE:${BUILD_NUMBER}'
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh '''
                sed -i "s|IMAGE_TAG|${BUILD_NUMBER}|g" k8s/backend-deployment.yaml
                sed -i "s|IMAGE_TAG|${BUILD_NUMBER}|g" k8s/frontend-deployment.yaml
                kubectl apply -f k8s/
                '''
            }
        }
    }
}
