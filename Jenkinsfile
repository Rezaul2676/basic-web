pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t basic-web:latest .
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                    docker rm -f basic-web || true
                '''
            }
        }

        stage('Deploy Website') {
            steps {
                sh '''
                    docker run -d \
                    --name basic-web \
                    --restart unless-stopped \
                    -p 80:80 \
                    basic-web:latest
                '''
            }
        }

        stage('Test Website') {
            steps {
                sh '''
                    sleep 5
                    curl -f http://localhost
                '''
            }
        }
    }

    post {

        success {
            echo "Website deployment successful!"
        }

        failure {
            echo "Deployment failed!"
        }
    }
}

