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
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    bat """
                    ${scannerHome}\\bin\\sonar-scanner.bat ^
                    -Dsonar.projectKey=iphone17-pro-web ^
                    -Dsonar.projectName=iphone17-pro-web ^
                    -Dsonar.sources=. ^
                    -Dsonar.login=%SONAR_TOKEN%
                    """
                }
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

        stage('OWASP Dependency Check') {
             steps {
               dependencyCheck additionalArguments: '--scan .', odcInstallation: 'DependencyCheck'
               dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
    }
}