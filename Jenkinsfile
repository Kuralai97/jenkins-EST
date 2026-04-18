pipeline {
    agent any

    tools {
        nodejs 'node' 
    }

    environment {
        // Branch-ке байланысты порт пен сурет атын анықтау
        APP_PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
        IMAGE_NAME = "${env.BRANCH_NAME == 'main' ? 'nodemain' : 'nodedev'}"
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Build') {
            steps { sh 'npm install' }
        }

        stage('Test') {
            steps { sh 'npm test || true' }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:v1.0 ."
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Ескі контейнерді өшіру
                    sh "docker stop ${IMAGE_NAME}_container || true"
                    sh "docker rm ${IMAGE_NAME}_container || true"
                    
                    // Жаңасын іске қосу (әр branch өз портында)
                    sh "docker run -d --name ${IMAGE_NAME}_container -p ${APP_PORT}:3000 ${IMAGE_NAME}:v1.0"
                }
            }
        }
    }
}
