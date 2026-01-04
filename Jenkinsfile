pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds') // Jenkins credential
        IMAGE_NAME = "remya92/website:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/RemyaKrishnanTR/website.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // No sudo needed inside the Jenkins container
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
                # Assuming deployment.yaml exists in your repo
                sudo k3s kubectl apply -f deployment.yaml
                '''
            }
        }
    }
}
