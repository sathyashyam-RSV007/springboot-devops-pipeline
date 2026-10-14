pipeline {
    agent any

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
    }
}