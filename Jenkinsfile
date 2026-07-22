pipeline {

    agent any


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


                    withCredentials([
                        string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')
                    ]) {


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




        stage('Docker Build') {

            steps {


                bat """

                docker build -t iphone17-pro-web .

                """


            }

        }


    }




    post {


        success {

            echo 'Pipeline completed successfully.'

        }



        failure {

            echo 'Pipeline failed.'

        }


    }


}