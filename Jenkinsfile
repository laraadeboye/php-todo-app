pipeline {
    agent any
    stages {
        stage ('build') {
            steps {
                script {
                    sh 'echo Building the application'
                }
            }
        }
         stage ('Test') {
            steps {
                script {
                    sh 'echo Testing the application'
                }
            }
        }
        stage ('Package') {
            steps {
                script {
                    sh 'echo Packaging the application'
                }
            }
        }
        stage ('Deploy') {
            steps {
                script {
                    sh 'echo Deploying the application'
                }
            }
        }
        stage ('Clean up') {
            steps {
                script {
                    sh 'echo Cleaning up the application'
                }
            }
        }
    }
}
