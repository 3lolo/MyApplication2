pipeline {
  agent {
    node {
      label 'master'
    }

  }
  stages {
    stage('Compile') {
      steps {
        bat 'gradlew asssembleDebug'
      }
    }
    stage('Unit test') {
      steps {
        sh './gradlew testDebugUnitTest testDebugUnitTest'
        junit '**/TEST-*.xml'
      }
    }
    stage('Build APK') {
      steps {
        sh './gradlew assembleDebug'
        archiveArtifacts '**/*.apk'
      }
    }
    stage('Static analysis') {
      steps {
        sh './gradlew lintDebug'
        androidLint(pattern: '**/lint-results-*.xml')
      }
    }
  }
  options {
    skipStagesAfterUnstable()
  }
}