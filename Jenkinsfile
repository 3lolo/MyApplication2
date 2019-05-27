pipeline {
  agent {
    node {
      label 'master'
    }

  }
  stages {
    stage('Compile') {
      steps {
        bat 'gradlew compileDemoDebugSources'
      }
    }
    stage('Unit test') {
      steps {
        bat 'gradlew testDemoDebugUnitTest'
        junit '**/TEST-*.xml'
      }
    }
    stage('Build APK') {
      steps {
        bat 'gradlew assembleDemoDebug'
        archiveArtifacts '**/*.apk'
      }
    }
    stage('Deploy') {
      steps {
        bat 'gradlew assembleFullRelease -PkeyAlias=\'test\' -PkeyPas=a123a123 -PstoreFile=\'C:/jenkins/keystore/keystore.jks\' -PstorePass=a123a123'
      }
    }
  }
  environment {
    GRADLE_USER_HOME = 'C:\\jenkins\\workflow-libs\\gradle'
  }
  post {
    always {
      echo 'I will always say Hello again!'
      mail(to: 'pozniack@gmail.com', subject: 'Oops!', body: "Build ${env.BUILD_NUMBER} failed; ${env.BUILD_URL}")
      emailext(body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}", recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}", to: 'wopozniack@gmail.com, pozniack@gmail.com')

    }

  }
  options {
    skipStagesAfterUnstable()
  }
}