pipeline { 
    agent any

    environment {
        SONAR_SCANNER_HOME = tool 'sonarqube-scanner-6.1.0'
    }

    stages {

        stage('Installing Dependencies') { 
            steps {
                sh 'npm install'
            }
        }

        stage('Unit Testing') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Code Coverage') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                sh 'npm run coverage'
                  }
            }
        }

        stage('SAST - SonarQube') {
            steps {
            timeout(time: 300, unit: 'SECONDS') {
            withSonarQubeEnv('sonar-qube-server') {
                withCredentials([
                    string(
                        credentialsId: 'sonarqube-token',
                        variable: 'SONAR_TOKEN'
                    )
                ]) {

                    sh """
                        $SONAR_SCANNER_HOME/bin/sonar-scanner \
                          -Dsonar.projectKey=Solar-System-Project \
                          -Dsonar.sources=app.js \
                          -Dsonar.host.url=http://43.204.142.38:9000 \
                          -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info \
                          -Dsonar.token=\$SONAR_TOKEN
                    """
                }
            }
                waitForQualityGate abortPipeline: true
            }
          }
        }
    }
}
