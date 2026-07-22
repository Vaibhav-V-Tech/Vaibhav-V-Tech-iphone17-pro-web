pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Repository checked out successfully'
            }
        }

        stage('SonarQube Scan') {
    steps {
        script {
            def scannerHome = tool 'SonarScanner'

            withSonarQubeEnv('sonarqube') {
                bat """
                ${scannerHome}\\bin\\sonar-scanner.bat ^
                -Dsonar.projectKey=iphone17-pro-web ^
                -Dsonar.projectName=iphone17-pro-web ^
                -Dsonar.sources=.
                """
            }
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