pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh '''export ${AWS_ACCESS_KEY_ID}
export ${AWS_SECRET_ACCESS_KEY}
export ${AWS_DEFAULT_REGION}
packer build centos7_opswerk.json'''
      }
    }
  }
}