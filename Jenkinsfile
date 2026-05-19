pipeline {
    agent any
    
    triggers {
        
        pollSCM('* * * * *') 
    }
    
    environment {
        // Jenkins Credentials'tan DockerHub şifrelerini güvenli bir şekilde çeker [cite: 76, 78]
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-id')
        DOCKER_IMAGE = 'zehranursarac/proje4-app:latest' 
    }
    
    stages {
        stage('Stage 1: Clone Repository') {
            steps {
                echo 'Proje GitHub üzerinden indiriliyor...'
                git branch: 'master', url: 'https://github.com/zehraanurss/SWE304-Pro4.git'
            }
        }
        stage('Stage 2: Build Project') {
            steps {
                echo 'Java projesi Gradle ile derleniyor (JDK 25)...'
                sh './gradlew clean bootJar'
            }
        }
        stage('Stage 3: Build Docker Image') {
            steps {
                echo 'Uygulama Dockerfile kullanılarak paketleniyor...'
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Stage 4: Login to Dockerhub') {
            steps {
                echo 'DockerHub bulut sistemine güvenli giriş yapılıyor...'
                
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Stage 5: Push Image to Dockerhub') {
            steps {
                echo 'Oluşan Docker imajı uzak depoya (DockerHub) yükleniyor...'
                sh 'docker push $DOCKER_IMAGE'
            }
        }
        stage('Stage 6: Deploy to Kubernetes') {
            steps {
                echo 'Uygulama Minikube (Kubernetes) üzerinde yayına alınıyor...'
                
                sh '/usr/local/bin/kubectl apply -f k8s.yaml --validate=false'
            }
        }
    }
}
