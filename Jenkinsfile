node {

    stage('Debug Workspace') {
        sh '''
          echo "===== WORKSPACE CONTENT ====="
          pwd
          ls -la
          echo "===== DIRECTORY TREE ====="
          find . -maxdepth 3 -type d
        '''
    }

    stage('Build Backend') {
        sh '''
          echo "=============================="
          echo " STEP 1: BUILDING BACKEND "
          echo "=============================="

          chmod +x backend/mvnw
          ./backend/mvnw clean package -DskipTests
        '''
    }
}
