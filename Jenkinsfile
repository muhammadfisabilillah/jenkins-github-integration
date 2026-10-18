pipeline {
    agent any

    environment {
        IMAGE_NAME = 'mfisabilillah/simple-app'
        REGISTRY_CREDENTIALS = 'dockerhub-credentials'
        // Menggunakan path umum Docker Desktop untuk user Windows
        DOCKER_PATH = 'C:\\Program Files\\Docker\\Docker\\resources\\docker.exe'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'echo "Build di Windows"'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    bat "\"${env.DOCKER_PATH}\" build -t ${env.IMAGE_NAME}:${env.BUILD_NUMBER} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${env.REGISTRY_CREDENTIALS}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        bat "\"${env.DOCKER_PATH}\" login -u %DOCKER_USER% -p %DOCKER_PASS%"
                        bat "\"${env.DOCKER_PATH}\" push ${env.IMAGE_NAME}:${env.BUILD_NUMBER}"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Selesai build'
        }
    }
}
