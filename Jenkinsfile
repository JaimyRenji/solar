pipeline {
    agent any

    environment {
        MONGO_URI      = "mongodb+srv://supercluster.d83jj.mongodb.net/superData"
        MONGO_DB_CREDS = credentials('mongo-db-credentials')
        MONGO_USERNAME = credentials('mongo-db-username')
        MONGO_PASSWORD = credentials('mongo-db-password')
    }

    options {
        // ...
    }

    stages {
        stage('Installing Dependencies')    { /* ... */ }

        stage('Dependency Scanning')        { /* ... */ }

        stage('Unit Testing') {
            options { retry(2) }
            steps {
                sh 'echo DB Creds    → $MONGO_DB_CREDS'
                sh 'echo Username    → $MONGO_USERNAME'
                sh 'echo Password    → $MONGO_PASSWORD'
                sh 'npm test'
            }
        }

        stage('Code Coverage') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                    sh 'npm run coverage'
                }
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true, testResults: 'test-results.xml'
            junit allowEmptyResults: true, testResults: 'dependency-check-junit.xml'
            publishHTML(
                allowMissing: true,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'coverage/lcov-report',
                reportFiles: 'index.html',
                reportName: 'Code Coverage HTML Report'
            )
            // Add other publishHTML steps here
        }
    }
}
