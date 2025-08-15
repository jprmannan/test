
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
                    bat 'mvn clean install' // Execute Maven goals
            }
        }
        stage('Test') {
                steps {
                    bat 'mvn test' // Execute Maven goals
            }
        }
        stage('Package') {
                steps {
                    bat 'mvn clean package' // Execute Maven goals
            }
        }
    }
}
