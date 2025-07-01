pipeline {
    agent any

    environment {
        IMAGE = "khyati8/flask-app:latest"
        CREDS_ID = "dockerhub-creds-id"
    }

    stages {
        stage('Clone Code') {
            steps {
                git 'https://github.com/khyati-source/my-app-flask.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: CREDS_ID, usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                        echo "$PASS" | docker login -u "$USER" --password-stdin
                        docker push $IMAGE
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl apply -f deployment.yaml"
            }
        }
    }
}

