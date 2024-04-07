pipeline {
    agent any

    stages {
        stage('create a directory') {
            steps {
                sh '''
                mkdir money
                rm -rf money
                '''
            }
        }
        stage('creating a file') {
            steps {
                sh '''
                touch money.txt
                rm -rf money.txt
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}