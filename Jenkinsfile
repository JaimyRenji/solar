pipeline {
    agent any

    parameters {
        string(
            name: 'USER_NAME',
            defaultValue: 'Hello, Jenkins from Parameter',
            description: 'Name of the user'
        )
    }

    environment {
        GREETING = 'Hello, Jenkins! from environment variable'
    }

    stages {
        stage('Print Basic String') {
            steps {
                echo 'Basic String Interpolation Examples:'
            }
        }
        stage('Interpolation with Variable') {
            steps {
                script {
                    def name = 'Jenkins User'
                    echo 'Hello, ${name}!'   // single quotes: no interpolation
                    echo "Hello, ${name}!"   // double quotes: interpolates
                }
            }
        }
        stage('Interpolation with Parameter') {
            steps {
                script {
                    echo "Hello, ${params.USER_NAME}"
                }
            }
        }
        stage('Interpolation with Environment Variable') {
            steps {
                script {
                    echo "Environment Variable Greeting: ${env.GREETING}"
                }
            }
        }
        stage('Interpolation with Expression') {
            steps {
                script {
                    def x = 5
                    def y = 10
                    echo "Sum of x and y is: ${x + y}"
                }
            }
        }
        stage('Complex Interpolation') {
            steps {
                script {
                    def list = [1, 2, 3]
                    echo "The list has ${list.size()} items: ${list.join(', ')}"
                }
            }
        }
        stage('Job Parameters') {
            steps {
                script {
                    def buildNumber = currentBuild.number
                    echo "This is build number ${buildNumber}"
                }
            }
        }
    }
}
