pipeline {
    agent any
    
    environment {
        TOMCAT_MANAGER_URL = 'http://localhost:8088/manager/text'
        TOMCAT_USERNAME = 'root'
        TOMCAT_PASSWORD = 'root'
        WAR_FILE = 'test.war'
    }

    stages {
        stage('Build') {
            steps {
                bat './gradlew bootWar'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    def warFilePath = bat(script: 'dir /B /S build\\*.war', returnStdout: true).trim()
                    def curlCommand = "curl -T ${warFilePath} ${TOMCAT_MANAGER_URL}/deploy?path=/${WAR_FILE} -u ${TOMCAT_USERNAME}:${TOMCAT_PASSWORD}"
                    bat "powershell ${curlCommand}"
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }

        failure {
            echo 'Deployment failed!'
        }
    }
}
