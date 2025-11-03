pipeline {
  agent any

  environment {
    TF_VERSION = "1.6.0"
    PATH = "$PATH:/tmp"
  }

  stages {
    stage('Checkout Code') {
      steps {
        git branch: 'main', url: 'https://github.com/kiranmayi-repo/ansible-portfolio.git'
      }
    }

    stage('Prepare tools (no sudo)') {
      steps {
        sh '''
        set -euo pipefail

        # install unzip if not present
        if ! command -v unzip >/dev/null 2>&1; then
          apt-get update -y || true
          apt-get install -y unzip || true
        fi

        # download Terraform binary
        cd /tmp
        if [ ! -f terraform ]; then
          wget -q https://releases.hashicorp.com/terraform/${TF_VERSION}/terraform_${TF_VERSION}_linux_amd64.zip
          unzip terraform_${TF_VERSION}_linux_amd64.zip
          chmod +x terraform
        fi

        terraform -version
        '''
      }
    }

    stage('Terraform Init') {
      steps {
        sh '''
        cd terraform
        /tmp/terraform init
        '''
      }
    }

    stage('Terraform Validate') {
      steps {
        sh '''
        cd terraform
        /tmp/terraform validate
        '''
      }
    }

    stage('Terraform Plan') {
      steps {
        sh '''
        cd terraform
        /tmp/terraform plan -out plan.tfplan
        '''
      }
    }

    stage('Terraform Apply') {
      steps {
        sh '''
        cd terraform
        /tmp/terraform apply -auto-approve plan.tfplan
        '''
      }
    }

    stage('Run Ansible Playbook') {
      steps {
        sh '''
        ansible-playbook -i inventory/dev playbooks/jenkins-install.yml
        ansible-playbook -i inventory/dev playbooks/nginx-deployment.yml
        '''
      }
    }
  }
}

