pipeline {
    agent any

    environment {
        IMAGE = "khyati8/flask-app:latest"
        CREDS_ID = "dockerhub-creds-id"
    }

    stages {
        stage('Clone Code') {
    steps {
        git credentialsId: 'my-github-username-token', url: 'https://github.com/khyati-source/my-flask-app-deploy.git', branch: 'main'
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
