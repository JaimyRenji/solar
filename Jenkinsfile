pipeline {
  agent any
  stages {
    stage('Installing Dependencies') {
      steps {
        sh 'npm install'
      }
    }
    stage('Dependency Scanning') {
      steps {
        sh 'npm audit'
      }
    }
    stage('Unit Testing') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'mongo-db-credentials',
          usernameVariable: 'MONGO_USERNAME',
          passwordVariable: 'MONGO_PASSWORD'
        )]) {
          sh 'npm test'
        }
      }
    }
    stage('Code Coverage') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'mongo-db-credentials',
          usernameVariable: 'MONGO_USERNAME',
          passwordVariable: 'MONGO_PASSWORD'
        )]) {
          sh 'npm run coverage'
        }
      }
    }
  }
}
