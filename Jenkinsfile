pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh '''export PACKER_LOG=1
export PACKER_LOG_PATH=$WORKSPACE/packer.log
echo "packer log path:" $PACKER_LOG_PATH
packer build centos7_opswerk.json'''
      }
    }
  }
}