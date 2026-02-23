pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/yourusername/yourrepo.git'
            }
        }

        stage('Build Backend') {
            steps {
                sh 'docker build -t backend-app ./backend'
            }
        }

        stage('Build Frontend') {
            steps {
                sh 'docker build -t frontend-app ./frontend'
            }
        }

        stage('Stop Old Containers') {
            steps {
                sh '''
                docker rm -f backend || true
                docker rm -f frontend || true
                '''
            }
        }

        stage('Run Containers') {
            steps {
                sh '''
                docker run -d --name backend \
                -p 8080:8080 \
                -e SPRING_DATASOURCE_URL=jdbc:mariadb://your-rds-endpoint:3306/student_db \
                -e SPRING_DATASOURCE_USERNAME=admin \
                -e SPRING_DATASOURCE_PASSWORD=sanika273112 \
                backend-app

                docker run -d --name frontend \
                -p 80:80 \
                frontend-app
                '''
            }
        }
    }
}
