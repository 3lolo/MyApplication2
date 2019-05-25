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
        bat 'Unit test'
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
        bat 'gradlew lintDebug'
      }
    }
    stage('Deploy') {
      steps {
        emailext(subject: 'Build', body: 'Success', replyTo: 'pozniack@gmail.com')
      }
    }
  }
  environment {
    GRADLE_USER_HOME = 'C:\\jenkins\\workflow-libs\\gradle'
  }
  options {
    skipStagesAfterUnstable()
  }
}