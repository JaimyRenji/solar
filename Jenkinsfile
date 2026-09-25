```groovy
pipeline {
    agent any

    environment {
        MONGO_URI      = "mongodb+srv://supercluster.d83jj.mongodb.net/superData"
        MONGO_DB_CREDS = credentials('mongo-db-credentials')
        MONGO_USERNAME = credentials('mongo-db-username')
        MONGO_PASSWORD = credentials('mongo-db-password')
    }

    stages {

        stage('Installing Dependencies') {
            steps {
                sh '''
                    export NODE_OPTIONS=--max-old-space-size=256
                    npm install --no-audit --no-fund --jobs=1
                '''
            }
        }

        stage('Dependency Scanning') {
            steps {
                sh 'npm audit || true'
            }
        }

        stage('Unit Testing') {
            options {
                retry(2)
            }

            steps {
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
            junit(
                allowEmptyResults: true,
                testResults: 'test-results.xml'
            )

            publishHTML(
                allowMissing: true,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'coverage/lcov-report',
                reportFiles: 'index.html',
                reportName: 'Code Coverage HTML Report'
            )
        }
    }
}
```
