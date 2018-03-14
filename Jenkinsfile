pipeline {
  agent any
  stages {
    stage('Environment') {
      steps {
        echo 'Installing Go v1.6 ...'
        sh '''GO_VERSION=1.6
GO_DIR=/usr/local
GO_TARGZ=go${GO_VERSION}.linux-amd64.tar.gz


export PATH=$GO_DIR/bin:$PATH
export GOROOT=$GO_DIR/go


if [ -x "$GO_DIR"/go ] ; then
    echo "Go $GO_VERSION already installed"
else
    # wget the binary archive
    wget https://dl.google.com/go/$GO_TARGZ

    # untar in $GO_DIR
    tar -C "$GO_DIR" -xzf  $GO_TARGZ
fi'''
      }
    }
    stage('Checkout') {
      steps {
        script {
          checkout([$class: 'GitSCM',
          branches: [[name: "*/master"]],
          doGenerateSubmoduleConfigurations: false,
          extensions: [[$class: 'CloneOption', noTags: true, shallow: true]],
          submoduleCfg: [],
          userRemoteConfigs: [[credentialsId: 'eyautomation', url: 'https://github.com/trilogy-group/ey-meh.git', permissions: 'READABLE']]])
        }
        
      }
    }
    stage('Test') {
      steps {
        sh 'go test'
      }
    }
  }
}