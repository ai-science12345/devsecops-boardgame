
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'devsecops-boardgame'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Check Tools') {
            steps {
                sh 'java -version'
                sh 'mvn -version'
                sh 'docker --version'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }

        stage('Docker Image Check') {
            steps {
                sh 'docker images ${IMAGE_NAME}'
            }
        }

        stage('Trivy Security Scan') {
            steps {
                sh '''
                    trivy image --exit-code 1 --severity HIGH,CRITICAL \
                        ${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }
    }
}
