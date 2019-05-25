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
        success {
           attachLog: true, body:
   """<p>EXECUTED: Job <b>\'${env.JOB_NAME}:${env.BUILD_NUMBER})\'
   </b></p><p>View console output at "<a href="${env.BUILD_URL}"> 
   ${env.JOB_NAME}:${env.BUILD_NUMBER}</a>"</p> 
     <p><i>(Build log is attached.)</i></p>""", 
    compressLog: true,
    recipientProviders: [[$class: 'DevelopersRecipientProvider'], 
     [$class: 'RequesterRecipientProvider']],
    replyTo: 'do-not-reply@company.com', 
    subject: "Status successs", 
    to: 'wopozniak@gmail.com pozniack@gmail.com'       }
    }
}
