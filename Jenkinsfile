pipeline {
  agent {
    node {
      label 'master'
    }

  }
  stages {
    stage('Compile') {
      steps {
        bat 'gradlew compileDebugSources'
      }
    }
    stage('Unit test') {
      steps {
        bat 'gradlew testDebugUnitTest testDebugUnitTest'
        junit '**/TEST-*.xml'
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