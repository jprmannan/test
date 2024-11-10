pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            steps {
                script {
                    git branch: 'main',
                        credentialsId: 'git-login-new',
                        url: 'https://github.com/username/repository.git'
                }
            }
        }
    }
}