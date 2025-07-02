pipeline {
    agent any

    tools {
        maven 'maven' // Define in Jenkins tool config
    }

    environment {
        GIT_REPO = 'https://github.com/Vamshi420/https-github.com-ashokitschool-maven-web-app.git'
        CREDENTIALS_ID = 'tomcat-cred' // Jenkins credentials ID
        TOMCAT_URL = 'http://3.110.54.234:8080' // Your Tomcat server
    }

    stages {
        stage('Clone from GitHub') {
            steps {
                git url: "${https://github.com/Vamshi420/https-github.com-ashokitschool-maven-web-app.git}"
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [
                    tomcat9(credentialsId: "${CREDENTIALS_ID}", 
                            path: '/', 
                            url: "${TOMCAT_URL}/manager/text")
                ], contextPath: '', war: 'target/*.war'
            }
        }
    }

    post {
        success {
            echo "✅ Build and Deployment successful!"
        }
        failure {
            echo "❌ Build or Deployment failed. Check logs."
        }
    }
}
