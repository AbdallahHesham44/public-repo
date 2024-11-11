pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/AbdallahHesham44/public-repo.git'  // Replace with your repository URL
            }
        }
       
               stage('Run file') {
            steps {
                script {
                    // Run the Docker container
                    sh 'chmod +x app1.sh'
                    sh './app1.sh'
                }
            }
        }
    }
    post {
       
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
