pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            step {
                script {
                    git branch: 'main',
                        credentialsId: 'git-login',
                        url: 'https://github.com/jprmannan/test.git'
                    }
				}
			}
		}
	}		