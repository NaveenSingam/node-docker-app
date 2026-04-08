pipeline {
    agent any

    environment {
        IMAGE_NAME = "node-docker-app"
        DOCKERHUB_REPO = "laxmi916/node-docker-app"
    }

    stages {

        stage('Checkout from GitHub') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/NaveenSingam/node-docker-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME:$BUILD_NUMBER .
                docker tag $IMAGE_NAME:$BUILD_NUMBER $DOCKERHUB_REPO:$BUILD_NUMBER
                '''
            }
        }

        // OPTIONAL (enable if needed)
        /*
        stage('Push Docker Image') {
            steps {
                sh '''
                docker login -u laxmi916 -p YOUR_PASSWORD
                docker push $DOCKERHUB_REPO:$BUILD_NUMBER
                '''
            }
        }
        */

        stage('Remove Old Container') {
            steps {
                sh 'docker rm -f node-app || true'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker run -d -p 3000:3000 --name node-app $IMAGE_NAME:$BUILD_NUMBER
                '''
            }
        }
    }
}
