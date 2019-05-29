pipeline {
  agent {
    node {
      label 'master'
    }

  }
  stages {
    stage('Compile') {
      steps {
        sh './gradlew clean'
        sh './gradlew compileDemoDebugSources'
      }
    }
    stage('Fabric') {
      steps {
        bat 'gradlew assembleFullRelease'
        bat 'gradlew crashlyticsUploadDistributionFullRelease'
      }
    }
  }
  environment {
    GRADLE_ROOT_HOME = 'var/jenkins_home/workflow-libs/gradle'
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