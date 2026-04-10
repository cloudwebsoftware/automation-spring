pipeline {
    agent any
    
     environment {
        GIT_URL = 'https://github.com/cloudwebsoftware/automation-spring.git'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "${GIT_URL}"
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    def image = docker.build("automation-spring:${env.BUILD_ID}")
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        docker.image("automation-spring:${env.BUILD_ID}").push('latest')
                        docker.image("automation-spring:${env.BUILD_ID}").push()
                    }
                }
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                sh """
                    sed -i 's/automation-spring:latest/automation-spring:${env.BUILD_ID}/g' k8s/app-deployment.yaml
                    kubectl apply -f k8s/ -n automationspring
                """
            }
        }
    }
}

