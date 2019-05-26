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
   post {
       always {
            echo 'I will always say Hello again!'
            
            emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}", 
                to:"wopozniack@gmail.com, pozniack@gmail.com"
            
        }
}
