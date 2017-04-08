pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Clone..'
                git 'https://github.com/id-den/jenkins-github.git'
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