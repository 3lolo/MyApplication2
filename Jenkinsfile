pipeline {
  agent {
    node {
      label 'master'
    }

  }
  stages {
    stage('Compile') {
      steps {
        bat 'gradlew clean'
        bat 'gradlew compileDemoDebugSources'
      }
    }
    stage('Deploy') {
      steps {
        bat 'gradlew assembleFullRelease -PkeyAlias=\'test\' -PkeyPas=a123a123 -PstoreFile=\'C:/jenkins/keystore/keystore.jks\' -PstorePass=a123a123'
      }
    }
    stage('Fabric') {
      steps {
        bat 'gradlew assembleFullDebug crashlyticsUploadDistributionFullDebug'
      }
    }
  }
  environment {
    GRADLE_ROOT_HOME = 'C:\\Jenkins\\workflow-libs\\gradle'
    GRADLE_USER_HOME = 'C:\\Jenkins\\workflow-libs\\gradle\\user'
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
