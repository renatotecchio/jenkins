pipeline {
  agent any
  options {
    timestamps()
  }
  stages {
    stage('Integration Test') {
      agent {
        docker {
          image 'gradle:8.2.0-jdk17-alpine'
            // Run the container on the node specified at the
            // top-level of the Pipeline, in the same workspace,
            // rather than on a new node entirely:
            reuseNode true
        }
      }
      steps {
        script {
          sh '''#!/bin/bash
            set -ev
            GO_VERSION="1.20.3"
            apt-get install unzip -y
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
