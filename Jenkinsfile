pipeline {
    agent any
    stages {
        stage ('Initial cleanup') {
            steps {
                dir ("${WORKSPACE}") {
                    deleteDir()
                }
            }
        }
         stage ('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/laraadeboye/php-todo-app.git'
            }
        }
        stage ('Prepare Dependencies') {
            steps {
                sh 'mv .env.sample .env'
                sh 'echo "DB_HOST=${DB_HOST}" >> .env'
                sh 'echo "DB_PORT=${DB_PORT}" >> .env'
                sh 'echo "DB_DATABASE=${DB_DATABASE}" >> .env'
                sh 'echo "DB_USERNAME=${DB_USERNAME}" >> .env'
                sh 'echo "DB_PASSWORD=${DB_PASSWORD}" >> .env'
                // Install composer dependencies
                sh 'composer install'

                // Run Laravel migrations and seed the database
                sh 'php artisan migrate'
                sh 'php artisan db:seed'

                // Generate Laravel application key
                sh 'php artisan key:generate'                
            }
        }              
    }
}
