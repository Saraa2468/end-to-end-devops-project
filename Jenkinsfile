pipeline {
    agent any

    environment {
        
        DOCKERHUB_USER = 'sarasalah24'
        APP_NAME       = 'art-store-app'
        IMAGE_NAME     = "${DOCKERHUB_USER}/${APP_NAME}"
        IMAGE_TAG      = "${BUILD_NUMBER}" 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Saraa2468/end-to-end-devops-project.git'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    echo "Building Docker Image: ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                    sh "docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Security Scan (Trivy)') {
            steps {
                script {
                    echo "Scanning Image for Vulnerabilities..."
                    sh "trivy image --severity HIGH,CRITICAL ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                        sh "echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin"
                        sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                        sh "docker push ${IMAGE_NAME}:latest"
                    }
                }
            }
        }

        stage('Deploy to K3s') {
            steps {
                
                withKubeConfig([credentialsId: 'k8s-config']) {
                    script {
                        echo "Deploying to Kubernetes Cluster..."
                        sh "kubectl apply -f k8s/deployment.yaml"
                        sh "kubectl set image deployment/${APP_NAME} ${APP_NAME}=${IMAGE_NAME}:${IMAGE_TAG}"
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Cleaning up local images..."
            sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG} || true"
        }
        success {
            echo "✅ Pipeline Finished Successfully!"
        }
        failure {
            echo " Pipeline Failed. Check the logs!"
        }
    }
}