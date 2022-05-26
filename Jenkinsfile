pipeline {
  agent any

  environment {
    deploymentName = "devsecops"
    containerName = "devsecops-container"
    serviceName = "devsecops-svc"
    imageName = "saad1969/numeric-app:${GIT_COMMIT}"
    applicationURL = "devsecops-demo111.brazilsouth.cloudapp.azure.com"
    applicationURI = "/increment/99"
  }

  stages {
    stage('Testing Slack') {
      steps {
        sh 'exit 0'
      }
    }

  }

  post {
    //    always { 
    //      junit 'target/surefire-reports/*.xml'
    //      jacoco execPattern: 'target/jacoco.exec'
    //      pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
    //      dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
    //      publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 'owasp-zap-report', reportFiles: 'zap_report.html', reportName: 'OWASP ZAP HTML Report', reportTitles: 'OWASP ZAP HTML Report'])

    // Use sendNotifications.groovy from shared library and provide current build result as parameter 
    //      sendNotification currentBuild.result
    //    }

    success {
      script {
        /* Use slackNotifier.groovy from shared library and provide current build result as parameter */
        env.failedStage = "none"
        env.emoji = ":white_check_mark: :tada: :thumbsup_all:"
        sendNotification currentBuild.result
      }
    }

    // failure {

    // }
  }

}