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
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_ID} ."
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'docker-hub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {
                        sh """
                            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                            docker push ${DOCKER_IMAGE}:${env.BUILD_ID}
                            docker tag ${DOCKER_IMAGE}:${env.BUILD_ID} ${DOCKER_IMAGE}:latest
                            docker push ${DOCKER_IMAGE}:latest
                        """
                    }
                }
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                sh """
                    sed -i 's|IMAGE_PLACEHOLDER|${DOCKER_IMAGE}:${env.BUILD_ID}|g' k8s/app-deployment.yaml
                    kubectl apply -f k8s/
                """
            }
        }
    }

    post {
        success {
            echo "✅ Build & Deployment Successful"
        }
        failure {
            echo "❌ Pipeline Failed"
        }
    }
}
