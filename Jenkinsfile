pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Repository checked out successfully'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t iphone17-pro-web .'
            }
        }
    }
}