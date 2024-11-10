pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            steps {
                script {
                    git branch: 'main',
                        credentialsId: 'git-login',
                        url: 'https://github.com/username/repository.git'
                }
            }
        }
    }
}