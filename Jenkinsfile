node {

    stage('Checkout') {
        checkout scm
    }

    stage('Build Backend') {
        sh '''
          echo "STEP 1: BUILDING BACKEND"
          chmod +x backend/mvnw
          ./backend/mvnw clean package -DskipTests
        '''
    }
}
