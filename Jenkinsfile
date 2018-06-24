pipeline {
  agent any
  stages {
    stage('build-ami') {
      steps {
        sh '''cd /root/sonar-scanner-3.2.0.1227-linux/bin

./sonar-scanner \\
  -Dsonar.projectKey=m00nbeam \\
  -Dsonar.organization=opswerk-github \\
  -Dsonar.sources=. \\
  -Dsonar.host.url=https://sonarcloud.io \\
  -Dsonar.login=4547c3161092741a3bf9ec954e0601cc173569d8'''
        sh '''export ${AWS_ACCESS_KEY_ID}
export ${AWS_SECRET_ACCESS_KEY}
export ${AWS_DEFAULT_REGION}

rm -f output.txt
/bin/packer build -force ${WORKSPACE}/centos7_opswerk.json 2>&1 | tee output.txt
AMI_ID=$(tail -2 output.txt | head -2 | awk \'match($0, /ami-.*/) { print substr($0, RSTART, RLENGTH) }\')
aws ssm put-parameter --name "ami_id" --value "${AMI_ID}" --type String --overwrite
'''
      }
    }
    stage('deploy-infra-dev') {
      steps {
        sh '''export ${AWS_ACCESS_KEY_ID}
export ${AWS_SECRET_ACCESS_KEY}
export ${AWS_DEFAULT_REGION}

AMI_ID=$(aws ssm get-parameters --names "ami_id" --output json | jq -r ".Parameters[] | .Value")

echo "ami_id = \\"${AMI_ID}\\"">>${WORKSPACE}/config/terraform/terraform.tfvars
cd ${WORKSPACE}/config/terraform
terraform init -input=false
terraform plan -out=tfplan -input=false
terraform apply -input=false tfplan'''
        cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenSuccess: true, cleanWhenUnstable: true, cleanupMatrixParent: true, deleteDirs: true)
      }
    }
  }
}