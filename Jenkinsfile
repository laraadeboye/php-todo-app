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
                    // Plot Lines of Code Metrics
                    plot(
                        csvFileName: 'phploc.csv', 
                        group: 'Code Metrics', 
                        numBuilds: '100',
                        title: 'A- Lines of Code', 
                        style: 'line',
                        yaxis: 'Count',
                        csvSeries: [
                            [
                                displayTableFlag: false,
                                file: 'build/logs/phploc.csv', 
                                inclusionFlag: 'INCLUDE_BY_STRING', 
                                url: ''
                            ]
                        ],
                        propertiesSeries: [
                            [name: 'Lines of Code', property: 'Lines of Code (LOC)'],
                            [name: 'Non-Comment Lines', property: 'Non-Comment Lines of Code (NCLOC)'],
                            [name: 'Comment Lines', property: 'Comment Lines of Code (CLOC)']
                        ]
                    )
                    
                    plot(
                        csvFileName: 'phploc.csv', 
                        group: 'Code Metrics', 
                        numBuilds: '100',
                        title: 'B - Structures Containers', 
                        style: 'line',
                        yaxis: 'Count',
                        csvSeries: [
                            [
                                displayTableFlag: false,
                                file: 'build/logs/phploc.csv', 
                                inclusionFlag: 'INCLUDE_BY_STRING', 
                                url: ''
                            ]
                        ],
                        propertiesSeries: [
                            [name: 'Directories', property: 'Directories'],
                            [name: 'Files', property: 'Files'],
                            [name: 'Namespaces', property: 'Namespaces']
                        ]
                    )
                    
                    plot(
                        csvFileName: 'phploc.csv', 
                        group: 'Code Metrics', 
                        numBuilds: '100',
                        title: 'C - Average Length', 
                        style: 'line',
                        yaxis: 'Count',
                        csvSeries: [
                            [
                                displayTableFlag: false,
                                file: 'build/logs/phploc.csv', 
                                inclusionFlag: 'INCLUDE_BY_STRING', 
                                url: ''
                            ]
                        ],
                        propertiesSeries: [
                            [name: 'Average Function Length', property: 'Average Function Length (LLOC)'],
                            [name: 'Average Class Length', property: 'Average Class Length'],
                            [name: 'Average Method Length', property: 'Average Method Length']
                        ]
                    )
                    
                    plot(
                        csvFileName: 'phploc.csv', 
                        group: 'Code Metrics', 
                        numBuilds: '100',
                        title: 'D - Relative Cyclomatic Complexity', 
                        style: 'line',
                        yaxis: 'Complexity',
                        csvSeries: [
                            [
                                displayTableFlag: false,
                                file: 'build/logs/phploc.csv', 
                                inclusionFlag: 'INCLUDE_BY_STRING', 
                                url: ''
                            ]
                        ],
                        propertiesSeries: [
                            [name: 'Complexity/LOC', property: 'Cyclomatic Complexity / Lines of Code'],
                            [name: 'Complexity/Number of Methods', property: 'Cyclomatic Complexity / Number of Methods']
                        ]
                    )
                    
                    plot(
                        csvFileName: 'phploc.csv', 
                        group: 'Code Metrics', 
                        numBuilds: '100',
                        title: 'E - Types of Classes', 
                        style: 'line',
                        yaxis: 'Count',
                        csvSeries: [
                            [
                                displayTableFlag: false,
                                file: 'build/logs/phploc.csv', 
                                inclusionFlag: 'INCLUDE_BY_STRING', 
                                url: ''
                            ]
                        ],
                        propertiesSeries: [
                            [name: 'Total Classes', property: 'Classes'],
                            [name: 'Abstract Classes', property: 'Abstract Classes'],
                            [name: 'Concrete Classes', property: 'Concrete Classes'],
                            [name: 'Final Classes', property: 'Final Classes']
                        ]
                    )
                    
                    plot(
                        csvFileName: 'phploc.csv', 
                        group: 'Code Metrics', 
                        numBuilds: '100',
                        title: 'F - Types of Methods', 
                        style: 'line',
                        yaxis: 'Count',
                        csvSeries: [
                            [
                                displayTableFlag: false,
                                file: 'build/logs/phploc.csv', 
                                inclusionFlag: 'INCLUDE_BY_STRING', 
                                url: ''
                            ]
                        ],
                        propertiesSeries: [
                            [name: 'Total Methods', property: 'Methods'],
                            [name: 'Non-Static Methods', property: 'Non-Static Methods'],
                            [name: 'Static Methods', property: 'Static Methods'],
                            [name: 'Public Methods', property: 'Public Methods'],
                            [name: 'Non-Public Methods', property: 'Non-Public Methods']
                        ]
                    )
                    
                    plot(
                        csvFileName: 'phploc.csv', 
                        group: 'Code Metrics', 
                        numBuilds: '100',
                        title: 'G - Types of Constants', 
                        style: 'line',
                        yaxis: 'Count',
                        csvSeries: [
                            [
                                displayTableFlag: false,
                                file: 'build/logs/phploc.csv', 
                                inclusionFlag: 'INCLUDE_BY_STRING', 
                                url: ''
                            ]
                        ],
                        propertiesSeries: [
                            [name: 'Total Constants', property: 'Constants'],
                            [name: 'Global Constants', property: 'Global Constants'],
                            [name: 'Class Constants', property: 'Class Constants']
                        ]
                    )
                    
                    plot(
                        csvFileName: 'phploc.csv', 
                        group: 'Code Metrics', 
                        numBuilds: '100',
                        title: 'H - Types of Functions', 
                        style: 'line',
                        yaxis: 'Count',
                        csvSeries: [
                            [
                                displayTableFlag: false,
                                file: 'build/logs/phploc.csv', 
                                inclusionFlag: 'INCLUDE_BY_STRING', 
                                url: ''
                            ]
                        ],
                        propertiesSeries: [
                            [name: 'Total Functions', property: 'Functions'],
                            [name: 'Named Functions', property: 'Named Functions'],
                            [name: 'Anonymous Functions', property: 'Anonymous Functions']
                        ]
                    )
                    
                    plot(
                        csvFileName: 'phploc.csv', 
                        group: 'Code Metrics', 
                        numBuilds: '100',
                        title: 'I - Testing', 
                        style: 'line',
                        yaxis: 'Count',
                        csvSeries: [
                            [
                                displayTableFlag: false,
                                file: 'build/logs/phploc.csv', 
                                inclusionFlag: 'INCLUDE_BY_STRING', 
                                url: ''
                            ]
                        ],
                        propertiesSeries: [
                            [name: 'Test Classes', property: 'Test Classes'],
                            [name: 'Test Methods', property: 'Test Methods']
                        ]
                    )
                    
                    // Plot code coverage
                    plot(
                        csvFileName: 'coverage.csv', 
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
                    )
                }
            }
        }
    }
}
