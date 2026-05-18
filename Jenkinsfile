pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-id')
        DOCKER_IMAGE = 'zehranursarac/proje4-app:latest' 
    }
    stages {
        stage('Stage 1: Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/zehraanurss/SWE304-Pro4.git'
            }
        }
        stage('Stage 2: Build Project') {
            steps {
                sh './gradlew clean bootJar'
            }
        }
        stage('Stage 3: Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Stage 4: Login to Dockerhub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Stage 5: Push Image to Dockerhub') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }
        stage('Stage 6: Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s.yaml'
            }
        }
    }
}
