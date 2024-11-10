pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            steps {
                script {
                    git branch: 'main',
                        credentialsId: 'Credential ID',
                        url: 'https://github.com/username/repository.git'
                }
            }
        }
    }
}