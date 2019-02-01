pipeline {
  agent {
    docker {
      image 'node:6-alpine'
      args '-p 3000:3000'
    }
  }
  post {
    failure {
      updateGitlabCommitStatus name: 'build', state: 'failed'
    }
    success {
      updateGitlabCommitStatus name: 'build', state: 'success'
    }
  }
  options {
    gitLabConnection('https://gitlab.com/tassanai/simple-node-js-react-npm-app.git')
  }
  triggers {
    gitlab(triggerOnPush: true, triggerOnMergeRequest: true, branchFilterType: 'All')
  }
  environment {
    CI = 'true'
  }
  stages {
    stage('Build') {
      steps {
        sh 'npm install -g yarn'
        sh 'yarn install'
      }
    }
    stage('Test') {
      steps {
        sh './jenkins/scripts/test.sh'
      }
    }
    stage('Deliver') {
      steps {
        sh './jenkins/scripts/deliver.sh'
        input message: 'Finished using the web site? (Click "Proceed" to continue)'
        sh './jenkins/scripts/kill.sh'
      }
    }
  }
}