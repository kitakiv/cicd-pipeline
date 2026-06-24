pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'chmod +x scripts/build.sh'
                sh "scripts/build.sh"
            }
        }
        stage('Test') {
            steps {
                sh 'chmod +x scripts/test.sh'
                sh 'scripts/test.sh'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}