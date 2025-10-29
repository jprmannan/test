pipeline {
    agent any

    environment {
        APP_NAME = "jv-1.0"
        WAR_FILE = "target\\jv-1.0.war"
        DEPLOY_SERVER = "windows-server"           // Jenkins node or remote WinRM host
        TOMCAT_HOME_BLUE = "E:\\Tomcat-blue"
        TOMCAT_HOME_GREEN = "E:\\Tomcat-green"
        DEPLOY_DIR_BLUE = "E:\\Tomcat-blue\\webapps"
        DEPLOY_DIR_GREEN = "E:\\Tomcat-green\\webapps"
        SERVICE_PORT_BLUE = "9000"
        SERVICE_PORT_GREEN = "9010"
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

        stage('Deploy to Green Tomcat') {
            steps {
                echo "Deploying to Green environment..."
                bat """
                if exist "%DEPLOY_DIR_GREEN%\\%APP_NAME%.war" del /Q "%DEPLOY_DIR_GREEN%\\%APP_NAME%.war"
                copy "%WORKSPACE%\\%WAR_FILE%" "%DEPLOY_DIR_GREEN%\\%APP_NAME%.war"
                """
            }
        }

        stage('Restart Green Tomcat') {
            steps {
                echo "Restarting Tomcat (Green)..."
                bat """
                net stop Tomcat-green
                timeout /t 5
                net start Tomcat-green
                """
            }
        }
        stage('Stop Old Blue Tomcat') {
            steps {
                echo "Stopping Blue Tomcat..."
                bat """
                net stop Tomcat-blue
                """
            }
        }
        stage('Cleanup Old Blue WAR') {
            steps {
                echo "Cleaning up old Blue deployment..."
                bat """
                del /Q "%DEPLOY_DIR_BLUE%\\%APP_NAME%.war"
                rmdir /S /Q "%DEPLOY_DIR_BLUE%\\%APP_NAME%"
                """
            }
        }
    }

    post {
        success {
            echo "✅ Blue-Green deployment completed successfully!"
        }
        failure {
            echo "❌ Deployment failed. Keeping Blue environment active."
        }
    }
}
