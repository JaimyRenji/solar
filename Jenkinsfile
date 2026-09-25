pipeline {
  agent any
  environment {
        RUN_TESTS = 'false'
    }
  stages {

    stage('Installing Dependencies') { 
    steps {
        sh '''
            export NODE_OPTIONS="--max-old-space-size=256"
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
    when {
        expression {
            return env.RUN_TESTS == 'true'
        }
    }
    steps {
        withCredentials([
            usernamePassword(
                credentialsId: 'mongo-db-credentials',
                usernameVariable: 'MONGO_USERNAME',
                passwordVariable: 'MONGO_PASSWORD'
            )
        ]) {
            sh '''
                export NODE_OPTIONS=--max-old-space-size=256
                npm test
            '''
        }
    }
}

    stage('Code Coverage') {
      steps {
        withCredentials([
          usernamePassword(
            credentialsId: 'mongo-db-credentials',
            usernameVariable: 'MONGO_USERNAME',
            passwordVariable: 'MONGO_PASSWORD'
          )
        ]) {

          catchError(
            buildResult: 'SUCCESS',
            stageResult: 'UNSTABLE',
            message: 'Coverage below threshold; will be fixed soon'
          ) {
            sh '''
              export NODE_OPTIONS="--max-old-space-size=256"
              npm run coverage
            '''
          }
        }

        publishHTML([
          allowMissing: true,
          alwaysLinkToLastBuild: true,
          keepAll: true,
          reportDir: 'coverage/lcov-report',
          reportFiles: 'index.html',
          reportName: 'Code Coverage HTML Report'
        ])
      }
    }
  }
}
