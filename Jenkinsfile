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
                net stop TomcatGreen
                timeout /t 5
                net start TomcatGreen
                """
            }
        }

        stage('Smoke Test Green') {
            steps {
                echo "Running smoke test on Green environment..."
                bat 'curl -f http://localhost:%SERVICE_PORT_GREEN%/%APP_NAME%/health || exit /b 1'
            }
        }

        stage('Switch Traffic to Green') {
            when {
                expression {
                    return input(message: "Switch traffic to Green?", ok: "Yes, deploy new version")
                }
            }
            steps {
                echo "Switching traffic to Green environment..."
                bat """
                REM Example: Update IIS or Nginx config here if needed
                REM Or rename Blue/Green folders to swap roles

                echo Switching traffic...
                """
            }
        }

        stage('Stop Old Blue Tomcat') {
            steps {
                echo "Stopping Blue Tomcat..."
                bat """
                net stop TomcatBlue
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
