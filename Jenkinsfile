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
        fabric(apiKey: 'ed9829a8396bceae47d4b5f246afc0a169c13096', buildSecret: 'a45cf0dbf4fdf79187c1335f45a99897ab4ff952926132e02d84d4093b7e3ab8', apkPath: 'C:\\jenkins\\workspace\\MyApplication2_master\\app\\build\\outputs\\apk\\full\\release\\app-full-release-unsigned.apk', testersEmails: 'pozniack@gmail.com', releaseNotesType: 'new build', releaseNotesParameter: '1.0.0', notifyTestersType: 'type', releaseNotesFile: 'no', testersGroup: 'none', organization: 'none', useAntStyleInclude: true)
      }
    }
  }
  environment {
    GRADLE_ROOT_HOME = 'C:\\jenkins\\workflow-libs\\gradle'
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