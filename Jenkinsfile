node {

    stage('Show Workspace Files') {
        sh '''
          echo "=== Workspace Root ==="
          ls -l
          echo "=== Subfolders ==="
          find . -maxdepth 2 -type d
        '''
    }

    stage('Build Backend') {
        sh '''
          echo "STEP 1: BUILDING BACKEND"
          chmod +x backend/mvnw
          ./backend/mvnw clean package -DskipTests
        '''
    }
}
