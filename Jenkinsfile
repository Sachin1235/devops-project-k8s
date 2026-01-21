pipeline {
  agent any
  stages {
    stage('Build Backend') {
      steps {
        dir('backend') {
          sh 'mvn clean package'
        }
      }
    }
    stage('Build Images') {
      steps {
        sh 'docker build -t localhost:8083/backend:1 backend'
        sh 'docker build -t localhost:8083/frontend:1 frontend'
      }
    }
    stage('Push Images') {
      steps {
        sh 'docker push localhost:8083/backend:1'
        sh 'docker push localhost:8083/frontend:1'
      }
    }
    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s/'
      }
    }
  }
}
