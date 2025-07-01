cat > Jenkinsfile <<EOF
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'yourdockerhubusername/flask-app:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/yourusername/my-flask-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t \$IMAGE_NAME .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                        echo "\$PASSWORD" | docker login -u "\$USERNAME" --password-stdin
                        docker push \$IMAGE_NAME
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
EOF

