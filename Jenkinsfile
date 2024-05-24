pipeline {
    agent any
    
    environment {
        TOMCAT_MANAGER_URL = 'http://172.31.200.65:8080/manager/text'
        TOMCAT_USERNAME = 'root'
        TOMCAT_PASSWORD = 'root'
        WAR_FILE = 'PrimeBilling.war'
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
                    def warFilePath = bat(script: 'dir /B /S target\\*.war', returnStdout: true).trim()
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
