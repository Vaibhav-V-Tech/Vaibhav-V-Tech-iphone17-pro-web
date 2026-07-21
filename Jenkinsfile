pipeline {
    agent any

    tools {
        sonarRunner 'SonarScanner'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Repository checked out successfully'
            }
        }

        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    bat '''
                    sonar-scanner ^
                    -Dsonar.projectKey=iphone17-pro-web ^
                    -Dsonar.projectName=iphone17-pro-web ^
                    -Dsonar.sources=.
                    '''
                }
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t iphone17-pro-web .'
            }
        }
    }
}