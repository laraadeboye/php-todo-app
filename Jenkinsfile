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
        // ... (previous stages remain the same)
        
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
                        yaxis: 'Lines of code',
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
                            [name: 'Comment Lines', property: 'Comment Lines of Code (CLOC)'],
                            [name: 'Logical Lines', property: 'Logical Lines of Code (LLOC)']
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
                        yaxis: 'Average Lines of Code',
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
                        yaxis: 'Cyclomatic Complexity by Structure',
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
                            [name: 'Concrete Classes', property: 'Concrete Classes']                            
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
                            [name: 'Methods', property: 'Methods'],
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
                            [name: 'Constants', property: 'Constants'],
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
