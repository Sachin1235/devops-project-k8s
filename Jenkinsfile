node {

    // ===== Global Variables =====
    def NEXUS_REGISTRY = "nexus:8083"
    def BACKEND_IMAGE = "${NEXUS_REGISTRY}/backend-app:latest"
    def FRONTEND_IMAGE = "${NEXUS_REGISTRY}/frontend-app:latest"
    def NEXUS_CREDS = "nexus-creds"

    stage('Build Backend') {
        sh '''
          echo "=============================="
          echo " STEP 1: BUILDING BACKEND "
          echo "=============================="

          chmod +x backend/mvnw
          ./backend/mvnw clean package -DskipTests
        '''
    }

    stage('Build Docker Images') {
        sh '''
          echo "=============================="
          echo " STEP 2: BUILD DOCKER IMAGES "
          echo "=============================="

          docker build -t ${BACKEND_IMAGE} backend/
          docker build -t ${FRONTEND_IMAGE} frontend/
        '''
    }

    stage('Login to Nexus') {
        withCredentials([usernamePassword(credentialsId: NEXUS_CREDS, usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
            sh '''
              echo "=============================="
              echo " STEP 3: LOGIN TO NEXUS "
              echo "=============================="

              echo $NEXUS_PASS | docker login ${NEXUS_REGISTRY} -u $NEXUS_USER --password-stdin
            '''
        }
    }

    stage('Push Images to Nexus') {
        sh '''
          echo "=============================="
          echo " STEP 4: PUSH IMAGES TO NEXUS "
          echo "=============================="

          docker push ${BACKEND_IMAGE}
          docker push ${FRONTEND_IMAGE}
        '''
    }

    stage('Deploy to Kubernetes (Kind)') {
        sh '''
          echo "=============================="
          echo " STEP 5: DEPLOY TO KUBERNETES "
          echo "=============================="

          kubectl apply -f k8s/
        '''
    }

    stage('Verify Deployment') {
        sh '''
          echo "=============================="
          echo " STEP 6: VERIFY PODS & SERVICES "
          echo "=============================="

          kubectl get pods
          kubectl get svc
        '''
    }
}
