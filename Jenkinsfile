pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh "react-scripts build"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}