pipeline {
  agent {
    node {
      label 'android'
    }

  }
  stages {
    stage('compile') {
      steps {
        sh './gradlew compileDebugSources'
      }
    }
  }
}