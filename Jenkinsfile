pipeline {
  agent any
  options {
    timestamps()
  }
  stages {
    stage('Integration Test') {
      steps {
        script {
          sh '''#!/bin/bash
            set -ev
            tf_version="1.3.6"
            GO_VERSION="1.20.3"
            OC_VERSION="4.11.0-0.okd-2022-10-28-153352"
            apt-get install unzip -y
            # golang version
            wget https://go.dev/dl/go$GO_VERSION.linux-amd64.tar.gz
            tar -C /usr/local -xzf go$GO_VERSION.linux-amd64.tar.gz
            export PATH=$PATH:/usr/local/go/bin
            go version
            source ./scripts/ci-before-install.sh
            source ./scripts/helpers.sh
            # authenticate to test-roks eu-gb (account 2578063 - IC4VG2CP JP-TOK)
            export IC_API_KEY="$API_KEY_AUTOMATED_TESTS"
            export IC_REGION="eu-de"
            export ENV="staging"
            auth $IC_API_KEY $IC_REGION "test"
            cd test/integration
            deleteReclamations
            # run tests
            go test -run TestIntegrationSmROKS -v -timeout 180m
            # cleanup after tests
            deleteReclamations
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
      def attachments = [
        [
          test: 'test',
          fallback: 'hei body',
          color: 'bad'
        ]
      ]
      slackSender(channel: "#devops", attachments: attachments)
    }
    cleanup {
      deleteDir()
    }
  }
}
