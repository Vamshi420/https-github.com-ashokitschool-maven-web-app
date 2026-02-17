pipeline {
    agent any

    tools {
        maven 'M3'
    }

    environment {
        SONAR_SERVER = 'SonarQube'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Vamshi420/https-github.com-ashokitschool-maven-web-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                    mvn sonar:sonar \
                    -Dsonar.projectKey=maven-web-app \
                    -Dsonar.host.url=http://3.108.190.250:9000/ \
                    -Dsonar.login=YOUR_SONAR_TOKEN
                    '''
                }
            }
        }

        stage("Quality Gate") {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(
                    credentialsId: 'tomcat-creds',
                    path: '',
                    url: 'http://15.206.27.10:8080/'
                )],
                contextPath: 'maven-web-app',
                war: 'webapp/target/*.war'
            }
        }
    }
}
