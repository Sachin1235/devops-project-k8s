pipeline {
    agent any

    environment {
        REGISTRY = "localhost:8083"
        BACKEND_IMAGE = "backend:1"
        FRONTEND_IMAGE = "frontend:1"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Backend') {
            steps {
                sh './backend/mvnw clean package'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker build -t $REGISTRY/$BACKEND_IMAGE backend'
                sh 'docker build -t $REGISTRY/$FRONTEND_IMAGE frontend'
            }
        }

        stage('Login to Nexus Registry') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus-docker-creds', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
                    sh 'echo $NEXUS_PASS | docker login $REGISTRY -u $NEXUS_USER --password-stdin'
                }
            }
        }

        stage('Push Images to Nexus') {
            steps {
                sh 'docker push $REGISTRY/$BACKEND_IMAGE'
                sh 'docker push $REGISTRY/$FRONTEND_IMAGE'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
}

