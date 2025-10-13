
pipeline {
    agent any
	    environment {
        TOMCAT_WEBAPPS = 'E:\\apache-tomcat-10.1.44\\webapps'
		WAR_NAME = 'jv-1.0.war'
		TOMCAT_HOME = 'E:\\apache-tomcat-10.1.44'
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
		stage('Deploy to Tomcat') {
                steps {
				// Copy WAR to remote Tomcat server
                 bat 'copy target\\%WAR_NAME% "%TOMCAT_WEBAPPS%\\"'
				 bat '"%TOMCAT_HOME%\\bin\\startup.bat"'
				
            }
        }
	}
            }
