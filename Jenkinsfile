pipeline {
    agent any

    environment {
        IMAGE_NAME = "archita392/dev"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Architaviswanathan/dev.git'
            }
        }

        stage('Build React App') {
            steps {
                sh 'chmod +x build.sh'
                sh './build.sh'  // Only build the React app
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t task3 .
                docker tag task3 $IMAGE_NAME
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                docker login -u archita392/dev -p 1234567890
                docker push $IMAGE_NAME
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f deploy.yaml --validate=false
                kubectl apply -f svc.yaml --validate=false
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment Successful! 🎉"
        }
        failure {
            echo "Deployment Failed! ❌"
        }
    }
}
