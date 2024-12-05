pipeline {
    agent any
    stages {
        stage('Initial Cleanup') {
            steps {
                dir("${WORKSPACE}") {
                    deleteDir()
                }
            }
        }
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/laraadeboye/php-todo-app.git'
            }
        }
        stage('Prepare Dependencies') {
            steps {
                script {
                    // Move .env.sample to .env and set environment variables
                    sh '''
                        mv .env.sample .env
                        echo "DB_HOST=${DB_HOST}" >> .env
                        echo "DB_PORT=${DB_PORT}" >> .env
                        echo "DB_DATABASE=${DB_DATABASE}" >> .env
                        echo "DB_USERNAME=${DB_USERNAME}" >> .env
                        echo "DB_PASSWORD=${DB_PASSWORD}" >> .env
                        echo "APP_ENV=${APP_ENV}" >> .env
                        echo "APP_DEBUG=${APP_DEBUG}" >> .env
                        echo "LOG_LEVEL=${LOG_LEVEL}" >> .env
                        echo "APP_KEY=${APP_KEY}" >> .env
                        echo "APP_URL=${APP_URL}" >> .env
                        echo "CACHE_DRIVER=${CACHE_DRIVER}" >> .env
                        echo "SESSION_DRIVER=${SESSION_DRIVER}" >> .env
                        echo "QUEUE_DRIVER=${QUEUE_DRIVER}" >> .env
                    '''
                    
                    // Create storage and bootstrap directories with appropriate permissions
                    sh '''
                        mkdir -p bootstrap/cache
                        mkdir -p storage/framework/sessions
                        mkdir -p storage/framework/views
                        mkdir -p storage/framework/cache                        
                        chown -R jenkins:jenkins bootstrap storage 
                        chmod -R 775 bootstrap storage 
                    '''

                    // Install Composer dependencies with error handling
                    sh '''
                        set -e                        
                        composer install 
                    '''                                    
                    
                    // Run Laravel artisan commands
                    sh '''                 
                        php artisan migrate --force
                        php artisan db:seed --force
                    '''
                }
            }
        }
        stage('Execute Unit Tests') {
            steps {
                sh './vendor/bin/phpunit'                
            }
        } 
        stage('Code Analysis') {
            steps {
                sh 'phploc app/ --log-csv build/logs/phploc.csv'                
            }
        }
        stage('Plot Code Coverage report') {
            steps {
                script {
                    // Plot phploc metrics
                    plot csvFileName: 'phploc.csv', 
                         group: 'Code Metrics', 
                         title: 'PHP Lines of Code', 
                         style: 'line',
                         csvSeries: [
                             [
                                 file: 'build/logs/phploc.csv', 
                                 inclusionFlag: 'OFF', 
                                 url: ''
                             ]
                         ]
                         propertiesSeries: [
                             [name: 'Lines of Code', property: 'Lines of Code (LOC)'],
                             [name: 'Logical Lines of Code', property: 'Logical Lines of Code (LLOC)']
                         ]
                    
                    // Plot code coverage
                    plot csvFileName: 'coverage.csv', 
                         group: 'Code Coverage', 
                         title: 'Test Coverage', 
                         style: 'bar',
                         csvSeries: [
                             [
                                 file: 'build/logs/clover.xml', 
                                 inclusionFlag: 'OFF', 
                                 url: ''
                             ]
                         ]
                }
            }
        }
    }
}
