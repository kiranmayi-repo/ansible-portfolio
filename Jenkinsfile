pipeline {
  agent any

  environment {
    PATH = "$PATH:/tmp"
  }

  stages {

    stage('Checkout Repo') {
      steps {
        git branch: 'main', url: 'https://github.com/kiranmayi-repo/ansible-portfolio.git'
      }
    }

    stage('Install unzip if missing') {
      steps {
        sh '''
        if ! command -v unzip >/dev/null; then
          echo "Installing unzip..."
          apt-get update -y || true
          apt-get install -y unzip || true
        fi
        '''
      }
    }

    stage('Install Terraform (no sudo)') {
      steps {
        sh '''
        cd /tmp

        if [ ! -f "terraform" ]; then
          wget https://releases.hashicorp.com/terraform/1.6.0/terraform_1.6.0_linux_amd64.zip -O terraform.zip
          unzip terraform.zip
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
        /tmp/terraform plan
        '''
      }
    }

    stage('Terraform Apply') {
      steps {
        sh '''
        cd terraform
        /tmp/terraform apply -auto-approve
        '''
      }
    }

    stage('Ansible Deployment') {
      steps {
        sh '''
        ansible-playbook -i inventory/dev playbooks/nginx-deployment.yml
        '''
      }
    }
  }
}
