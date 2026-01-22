node {

    stage('Manual Checkout') {
        sh '''
          echo "Manual Git Checkout..."
          rm -rf repo
          git clone https://github.com/Sachin1235/devops-project-k8s.git repo
          cd repo
        '''
    }

    stage('Build Backend') {
        sh '''
          cd repo
          echo "STEP 1: BUILDING BACKEND"
          chmod +x backend/mvnw
          ./backend/mvnw clean package -DskipTests
        '''
    }
}
