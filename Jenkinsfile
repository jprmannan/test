
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
	    stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [
                    tomcat10(
                        credentialsId: 'tomcat-user',
                        url: 'http://localhost:8080'
                    )
                ],
                // Deploy to the root context (accessible at /).
                // Change to 'your-app-name' to deploy to http://host:8080/your-app-name.
                contextPath: '',
                war: '**/target/*.war'
            }
        }
	post {
        always {
            // Actions to perform after the build, regardless of success or failure
            echo 'Build finished.'
        }
        success {
            echo 'Build successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
