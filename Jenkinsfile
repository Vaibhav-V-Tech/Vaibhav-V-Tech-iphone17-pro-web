pipeline {

    agent any


    environment {

        SCANNER_HOME = tool 'SonarScanner'

    }


    stages {


        stage('Checkout') {

            steps {

                checkout scm

                echo "Repository checked out successfully"

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


                    waitForQualityGate abortPipeline: false


                }

            }

        }




        stage('OWASP Dependency Check') {


            steps {


                withCredentials([
                    string(credentialsId: 'nvd_api_key', variable: 'NVD_KEY')
                ]) {


                    dependencyCheck(

                        odcInstallation: 'DependencyCheck',

                        additionalArguments: """
                        --scan .
                        --format HTML
                        --format XML
                        --out dependency-check-report
                        --nvdApiKey %NVD_KEY%
                        """,

                        stopBuild: false

                    )


                }


                dependencyCheckPublisher(

                    pattern: '**/dependency-check-report.xml',

                    stopBuild: false

                )


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


        always {


            archiveArtifacts(

                artifacts: 'dependency-check-report/**/*',

                allowEmptyArchive: true

            )


        }



        success {


            echo "Pipeline completed successfully"

        }



        failure {


            echo "Pipeline failed"

        }


    }


}