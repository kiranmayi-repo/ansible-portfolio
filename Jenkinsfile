pipeline {
  agent any

  environment {
    PATH = "$PATH:/tmp"
  }

  stages {

    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/kiranmayi-repo/ansible-portfolio.git'
      }
    }

    stage('Install unzip if missing') {
      steps {
        sh '''
        set -e
        if ! command -v unzip >/dev/null; then
          echo "Installing unzip"
          apt-get update -y
          apt-get install -y unzip
        fi
        '''
      }
    }

    stage('Install Terraform (user space)') {
      steps {
        sh '''
        set -e
        cd /tmp

        if [ ! -f terraform ]; then
          echo "Downloading Terraform..."
          wget -q https://releases.hashicorp.com/terraform/1.6.0/terraform_1.6.0_linux_amd64.zip
          unzip -o terraform_1.6.0_linux_amd64.zip
        fi

        chmod +x terraform
        export PATH=$PATH:/tmp
        terraform version
        '''
      }
    }

    stage('Terraform Init') {
      steps {
        sh '''
        export PATH=$PATH:/tmp
        terraform init
        '''
      }
    }

    stage('Terraform Validate') {
      steps {
        sh '''
        export PATH=$PATH:/tmp
        terraform validate
        '''
      }
    }

    stage('Terraform Plan') {
      steps {
        sh '''
        export PATH=$PATH:/tmp
        terraform plan
        '''
      }
    }

    stage('Terraform Apply') {
      steps {
        sh '''
        export PATH=$PATH:/tmp
        terraform apply -auto-approve
        '''
      }
    }

    stage('Ansible Deployment') {
      steps {
        sh '''
        ansible-playbook -i inventory/dev playbooks/jenkins-install.yml
        '''
      }
    }

  }
}
