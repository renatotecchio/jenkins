pipeline {
  agent { label 'ubuntu' }
  options {
    timestamps()
  }
  stages {
    stage('Integration Test') {
      steps {
        script {
          sh '''#!/bin/bash
            set -ev
            GO_VERSION="1.20.3"
            apt-get install unzip -y
            apt-get install wget -y
            # golang version
            wget https://go.dev/dl/go$GO_VERSION.linux-amd64.tar.gz
            tar -C /usr/local -xzf go$GO_VERSION.linux-amd64.tar.gz
            export PATH=$PATH:/usr/local/go/bin
            go version
          '''
        }
      }          
    }
  }
  post {
    success {
      echo 'Success notification'
    }
    failure {
      echo 'Failure notification'
      slackSend(channel: "#devops", message: "PipelineRun #${BUILD_NUMBER} in Pipeline ${JOB_BASE_NAME} has FAILED\nJenkins View: Default\nURL Link: ${BUILD_URL}", color: "#FF0000")
    }
    cleanup {
      deleteDir()
    }
  }
}
