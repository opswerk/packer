pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh 'packer build centos7_opswerk.json'
      }
    }
  }
}