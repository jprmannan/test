
pipeline {
    agent any
	    environment {
        TOMCAT_URL = 'http://localhost:8080/manager/text/deploy?path=/myapp&update=true'
        CREDENTIALS_ID = 'tomcat-user'
    }
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
