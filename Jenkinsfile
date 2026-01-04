pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = "remya92/website:latest"
        KUBECONFIG = "/root/.kube/config"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/RemyaKrishnanTR/website.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh '''
                echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                docker push $IMAGE_NAME
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                export KUBECONFIG=/root/.kube/config
                kubectl apply -f deployment.yaml
                '''
            }
        }
    }
}
