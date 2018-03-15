pipeline {
  agent any
  stages {
    stage('Environment') {
      steps {
        echo 'Installing Go v1.6 ...'
        sh '''
GO_VERSION=1.6
GO_DIR=$HOME/gopath
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
          userRemoteConfigs: [[credentialsId: 'eyautomation', url: 'https://github.com/trilogy-group/ey-meh.git', permissions: 'READABLE',destinationDir: '$HOME/gopath/src/github.com/engineyard/meh']]])
        }
        
      }
    }
    stage('Test') {
      steps {
        sh '''export GOPATH=$HOME/gopath
export PATH=$HOME/gopath/bin:$PATH

go get github.com/DATA-DOG/godog/cmd/godog
go get github.com/ess/kennel

go test'''
      }
    }
  }
}