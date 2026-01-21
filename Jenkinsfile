pipeline {
    agent any

    environment {
        REGISTRY = "localhost:8083/docker-repo"
        BACKEND_IMAGE = "backend:1"
        FRONTEND_IMAGE = "frontend:1"
        NEXUS_CREDS = credentials('nexus-creds')
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh './mvnw clean package -DskipTests'
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                sh "docker build -t $REGISTRY/$BACKEND_IMAGE backend"
                sh "docker build -t $REGISTRY/$FRONTEND_IMAGE frontend"
            }
        }

        stage('Login to Nexus Registry') {
            steps {
                sh """
                echo $NEXUS_CREDS_PSW | docker login $REGISTRY \
                -u $NEXUS_CREDS_USR --password-stdin
                """
            }
        }

        stage('Push Images to Nexus') {
            steps {
                sh "docker push $REGISTRY/$BACKEND_IMAGE"
                sh "docker push $REGISTRY/$FRONTEND_IMAGE"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl apply -f k8s/"
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed!"
        }
    }
}
