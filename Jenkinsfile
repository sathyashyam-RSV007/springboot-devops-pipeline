pipeline {
    agent any

    environment {
        IMAGE_NAME = "sathyahub123/springboot-darkmode"
        CONTAINER_NAME = "darkmode-container"
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
            }
        }
        stage('Deploy Application') {
            steps {
                sh 'docker rm -f $CONTAINER_NAME || true'
                sh 'docker run -d --name $CONTAINER_NAME --restart unless-stopped -p 8081:8080 $IMAGE_NAME:$BUILD_NUMBER'
                sh 'docker image prune -f'
            }
        }
        stage('Verify Deployment Health') {
            steps {
                sh '''
                    for i in 1 2 3 4 5 6; do
                        sleep 5
                        if curl -s http://localhost:8081/actuator/health | grep -q '"status":"UP"'; then
                            echo "Container is UP and healthy!"
                            exit 0
                        fi
                        echo "Waiting for application startup (attempt $i)..."
                    done
                    echo "Deployment health check failed!"
                    docker logs --tail 30 $CONTAINER_NAME
                    exit 1
                '''
            }
        }
    }
}