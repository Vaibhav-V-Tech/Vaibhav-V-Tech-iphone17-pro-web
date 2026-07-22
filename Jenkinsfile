pipeline {
    agent any

    tools {
    dependencyCheck 'DependencyCheck'
}

    environment {
        SCANNER_HOME = tool 'SonarScanner'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
                echo 'Repository checked out successfully'
            }
        }

        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                        bat """
                        %SCANNER_HOME%\\bin\\sonar-scanner.bat ^
                        -Dsonar.projectKey=iphone17-pro-web ^
                        -Dsonar.projectName=iphone17-pro-web ^
                        -Dsonar.sources=. ^
                        -Dsonar.token=%SONAR_TOKEN%
                        """
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('OWASP Dependency Check') {
            steps {

                dependencyCheck(
                    odcInstallation: 'DependencyCheck',
                    additionalArguments: '''
                        --scan .
                        --format ALL
                        --out dependency-check-report
                    ''',
                    stopBuild: false
                )

                dependencyCheckPublisher(
                    pattern: 'dependency-check-report/dependency-check-report.xml'
                )
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t iphone17-pro-web .'
            }
        }

    }

    post {

        always {
            archiveArtifacts artifacts: 'dependency-check-report/**/*', allowEmptyArchive: true
        }

        success {
            echo 'Pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}