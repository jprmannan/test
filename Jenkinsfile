
pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            steps {
                  git branch: 'main',
                        credentialsId: 'git-login',
                        url: 'https://github.com/jprmannan/test.git'
                }
            }
        stage('Build') {
                steps {
                    sh 'mvn clean install' // Execute Maven goals
            }
        }
    }
}
