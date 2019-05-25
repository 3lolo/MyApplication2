pipeline {
  agent {
    node {
      label 'master'
    }

  }
  stages {
    stage('Unit test') {
      steps {
        ws(dir: 'C:\\jenkins\\workspace\\MyApplication2_master')
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