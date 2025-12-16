pipeline {
    agent any

    environment {
        IMAGE_NAME = "angular-app"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image for branch: ${env.BRANCH_NAME}"
                sh "docker build -t ${IMAGE_NAME}:${env.BRANCH_NAME} ."
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh '''
                        docker stop angular-staging || true
                        docker rm angular-staging || true
                        docker run -d -p 4200:80 --name angular-staging angular-app:main
                        '''
                    }

                    if (env.BRANCH_NAME == 'master') {
                        sh '''
                        docker stop angular-prod || true
                        docker rm angular-prod || true
                        docker run -d -p 80:80 --name angular-prod angular-app:master
                        '''
                    }
                }
            }
        }
    }
}
