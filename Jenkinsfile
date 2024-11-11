pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/AbdallahHesham44/public-repo.git', branch: 'main'
            }
        }
        stage('Run file') {
            steps {
                script {
                    // Debugging: List files in workspace
                    sh 'ls -l'
                    // Make the script executable and run it
                    sh 'chmod +x app1.sh'
                    sh './app1.sh'
                }
            }
        }
    }
}
