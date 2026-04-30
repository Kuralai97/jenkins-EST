pipeline {
    agent any

    tools {
        nodejs 'node'
    }

    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Docker build') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh 'docker build -t nodemain:v1.0 .'
                    } else {
                        sh 'docker build -t nodedev:v1.0 .'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh '''
                            docker stop nodemain || true
                            docker rm nodemain || true
                            docker run -d --name nodemain -p 3000:3000 nodemain:v1.0
                        '''
                    } else {
                        sh '''
                            docker stop nodedev || true
                            docker rm nodedev || true
                            docker run -d --name nodedev -p 3001:3000 nodedev:v1.0
                        '''
                    }
                }
            }
        }
    }
}
