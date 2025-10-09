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
        stage ('Build') {
		    step {
			 sh 'mvn install' //build
			     }
	        }
		stage ('Git test') {
		    step {
			 sh 'mvn test' //test
			}
		}
		stage ('Git package') {
		    step {
			 sh 'mvn package' //generating package
			     }
			}
        }
    }
 }