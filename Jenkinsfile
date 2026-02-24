pipeline {
    agent any

    environment {
        RDS_URL = "jdbc:mariadb://database-1.cx0g4sye0xfn.us-east-2.rds.amazonaws.com:3306/student_db?sslMode=REQUIRED"
    }

    stages {

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t backend-app ./backend'
            }
        }

        stage('Build Frontend Image') {
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

        stage('Run Backend') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'rds-creds',
                    usernameVariable: 'DB_USER',
                    passwordVariable: 'DB_PASS'
                )]) {
                    sh '''
                    docker run -d --name backend \
                    -p 8080:8080 \
                    -e SPRING_DATASOURCE_URL=$RDS_URL \
                    -e SPRING_DATASOURCE_USERNAME=$DB_USER \
                    -e SPRING_DATASOURCE_PASSWORD=$DB_PASS \
                    backend-app
                    '''
                }
            }
        }

        stage('Run Frontend') {
            steps {
                sh '''
                docker run -d --name frontend \
                -p 80:80 \
                frontend-app
                '''
            }
        }
    }
}
