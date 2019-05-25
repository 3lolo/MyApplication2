pipeline {
  agent {
    node {
      label 'master'
    }

  }
  stages {
    stage('Unit test') {
      steps {
        bat 'gradlew testDebugUnitTest testDebugUnitTest'
        junit '**/TEST-*.xml'
        sh 'ls -la ${pwd()}'
      }
    }
    stage('Build APK') {
      steps {
        bat 'gradlew assembleDebug'
        archiveArtifacts '**/*.apk'
      }
    }
    stage('Static analysis') {
      steps {
        androidLint(pattern: '**/lint-results-*.xml')
        bat 'gradlew lintDebug'
      }
    }
  }
  options {
    skipStagesAfterUnstable()
  }
}