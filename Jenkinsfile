pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'abdallah1312/test'  // Your Docker Hub repository
        DOCKER_TAG = 'latest'
        K8S_DEPLOYMENT_FILE = 'k8s/deployment.yaml'  // Path to Kubernetes deployment YAML file
    }
    stages {
        stage('Checkout') {
            steps {
                // Use named parameters for the git step
                git url: 'https://github.com/AbdallahHesham44/public-repo.git', branch: 'dev'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image with a specific tag
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    // Use withCredentials to securely access Docker Hub credentials
                    withCredentials([usernamePassword(credentialsId: 'docker-cred', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Log in to Docker Hub using the credentials
                        sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"

                        // Push the Docker image to Docker Hub
                        sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                    }
                }
            }
        }
        stage('Update Kubernetes Deployment YAML') {
            steps {
                script {
                    // Update the image name and tag in the Kubernetes deployment YAML file
                    sh """
                    sed -i 's|image:.*|image: ${DOCKER_IMAGE}:${DOCKER_TAG}|g' ${K8S_DEPLOYMENT_FILE}
                    """
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply the updated Kubernetes deployment YAML to the cluster
                    sh "kubectl apply -f ${K8S_DEPLOYMENT_FILE}"
                }
            }
        }
    }
    post {
        always {
            script {
                // Log out of Docker Hub
                sh "docker logout"
            }
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
