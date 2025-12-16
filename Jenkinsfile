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
                echo "Building image for branch: ${BRANCH_NAME}"
                bat "docker build -t %IMAGE_NAME%:%BRANCH_NAME% ."
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        bat """
                        docker rm -f angular-main || exit 0
                        docker run -d -p 4200:80 --name angular-main %IMAGE_NAME%:main
                        """
                    }

                    if (env.BRANCH_NAME == 'master') {
                        bat """
                        docker rm -f angular-master || exit 0
                        docker run -d -p 4300:80 --name angular-master %IMAGE_NAME%:master
                        """
                    }
                }
            }
        }
    }
}
