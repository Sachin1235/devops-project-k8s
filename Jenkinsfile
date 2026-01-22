node {

    def REPO_URL = "https://github.com/Sachin1235/devops-project-k8s.git"

    stage('Manual Git Clone') {
        sh '''
          echo "=============================="
          echo " STEP 0: MANUAL GIT CLONE "
          echo "=============================="

          rm -rf project
          git clone ${REPO_URL} project
          ls -la project
        '''
    }

    stage('Build Backend') {
        sh '''
          echo "=============================="
          echo " STEP 1: BUILDING BACKEND "
          echo "=============================="

          cd project
          chmod +x backend/mvnw
          ./backend/mvnw clean package -DskipTests
        '''
    }

    stage('Build Docker Images') {
        sh '''
          echo "=============================="
          echo " STEP 2: BUILD DOCKER IMAGES "
          echo "=============================="

          cd project
          docker build -t nexus:8083/backend-app:latest backend/
          docker build -t nexus:8083/frontend-app:latest frontend/
        '''
    }

    stage('Login to Nexus') {
        withCredentials([usernamePassword(credentialsId: 'nexus-creds', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
            sh '''
              echo "=============================="
              echo " STEP 3: LOGIN TO NEXUS "
              echo "=============================="

              echo $NEXUS_PASS | docker login nexus:8083 -u $NEXUS_USER --password-stdin
            '''
        }
    }

    stage('Push Images') {
        sh '''
          echo "=============================="
          echo " STEP 4: PUSH IMAGES "
          echo "=============================="

          docker push nexus:8083/backend-app:latest
          docker push nexus:8083/frontend-app:latest
        '''
    }

    stage('Deploy to Kubernetes') {
        sh '''
          echo "=============================="
          echo " STEP 5: DEPLOY TO K8S "
          echo "=============================="

          cd project
          kubectl apply -f k8s/
        '''
    }

    stage('Verify Pods') {
        sh '''
          echo "=============================="
          echo " STEP 6: VERIFY PODS "
          echo "=============================="

          kubectl get pods
          kubectl get svc
        '''
    }
}
