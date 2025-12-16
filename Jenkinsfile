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
                bat "docker build -t %IMAGE_NAME%:%BRANCH_NAME% ."
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        bat """
                        docker stop angular-staging || exit 0
                        docker rm angular-staging || exit 0
                        docker run -d -p 4200:80 --name angular-staging angular-app:main
                        """
                    }

                    if (env.BRANCH_NAME == 'master') {
                        bat """
                        docker stop angular-prod || exit 0
                        docker rm angular-prod || exit 0
                        docker run -d -p 80:80 --name angular-prod angular-app:master
                        """
                    }
                }
            }
        }
    }
}
