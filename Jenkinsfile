pipeline {
    agent any

    environment {
        IMAGE_NAME = "sathyahub123/springboot-darkmode"
        CONTAINER_NAME = "darkmode-container"
        KUBECONFIG = "/var/lib/jenkins/.kube/config"
    }

    stages {
        stage('Compile Code') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Package JAR') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER -t $IMAGE_NAME:latest .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
                    sh 'docker push $IMAGE_NAME:latest'
                    sh 'docker logout'
                }
                sh 'docker rmi $IMAGE_NAME:$BUILD_NUMBER || true'
                sh 'docker image prune -f'
            }
        }
        stage('Deploy to K3s') {
            steps {
                sh 'kubectl apply -f k8s/service.yaml'
                sh 'sed "s|:latest|:${BUILD_NUMBER}|g" k8s/deployment.yaml | kubectl apply -f -'
            }
        }
        stage('Verify K3s Rollout & Health') {
            steps {
                sh '''
                    kubectl rollout status deployment/pick-your-sky --timeout=180s
                    curl -sf http://localhost:30081/actuator/health | grep -q '"status":"UP"'
                    echo "K3s rollout complete and application is UP!"
                '''
            }
        }
    }
}
