pipeline {
  agent any
  stages {
    stage('build-ami') {
      steps {
        sh '''export ${AWS_ACCESS_KEY_ID}
export ${AWS_SECRET_ACCESS_KEY}
export ${AWS_DEFAULT_REGION}

/bin/packer build ${WORKSPACE}/centos7_opswerk.json 2>&1 | tee output.txt
AMI_ID=$(tail -2 output.txt | head -2 | awk \'match($0, /ami-.*/) { print substr($0, RSTART, RLENGTH) }\')
aws ssm put-parameter --name "ami_id" --value "${AMI_ID}" --type String
'''
      }
    }
    stage('deploy-infra-dev') {
      steps {
        sh '''export ${AWS_ACCESS_KEY_ID}
export ${AWS_SECRET_ACCESS_KEY}
export ${AWS_DEFAULT_REGION}
AMI_ID=$(aws ec2 describe-images --filters "Name=tag:Name,Values=centos" --query \'Images[*].{ID:ImageId}\' --region us-east-1 --output text)
aws ssm put-parameter --name "ami_id" --value "${AMI_ID}" --type String
echo ${AMI_ID}
cd ${WORKSPACE}/config/terraform
terraform init
terraform apply'''
      }
    }
  }
}