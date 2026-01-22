node {

    stage('Build Backend') {
        sh '''
          echo "=============================="
          echo " STEP 1: BUILDING BACKEND "
          echo "=============================="
          chmod +x backend/mvnw
          ./backend/mvnw clean package -DskipTests
        '''
    }

    // Next stages will be added later step-by-step
    // stage('Build Docker Images') { }
    // stage('Push to Nexus') { }
    // stage('Deploy to Kubernetes') { }

}
