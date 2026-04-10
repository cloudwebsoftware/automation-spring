pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    environment {
        GIT_URL = 'https://github.com/cloudwebsoftware/automation-spring.git'
        DOCKER_IMAGE = 'anoopdubey/automation-spring'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: "${GIT_URL}"
            }
        }

        stage('Build (Maven)') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    bat "docker build -t ${DOCKER_IMAGE}:${env.BUILD_ID} ."
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    bat "docker login -u anoopdubey -p YOUR_TOKEN"
                    bat "docker push ${DOCKER_IMAGE}:${env.BUILD_ID}"
                    bat "docker tag ${DOCKER_IMAGE}:${env.BUILD_ID} ${DOCKER_IMAGE}:latest"
                    bat "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                bat "kubectl apply -f k8s/"
            }
        }
    }
}