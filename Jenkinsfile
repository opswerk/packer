pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh '''export ${AWS_ACCESS_KEY_ID}
export ${AWS_SECRET_ACCESS_KEY}
export ${AWS_DEFAULT_REGION}
ls -altr
pwd
echo $AWS_ACCESS_KEY_ID
echo $AWS_SECRET_ACCESS_KEY
echo $AWS_DEFAULT_REGION
/bin/packer build ${WORKSPACE}/centos7_opswerk.json'''
      }
    }
  }
}