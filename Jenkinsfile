pipeline {
  agent any

  environment {
<<<<<<< HEAD
    PATH = "$PATH:/tmp"
=======
    PATH = "$PATH:/usr/local/bin"
>>>>>>> fa6f9a0 (Fix Jenkins pipeline automation)
  }

  stages {

<<<<<<< HEAD
    stage('Checkout Repo') {
=======
    stage('Install unzip if missing') {
>>>>>>> fa6f9a0 (Fix Jenkins pipeline automation)
      steps {
        sh '''
        if ! command -v unzip >/dev/null; then
          echo "Installing unzip..."
          sudo apt-get update -y
          sudo apt-get install -y unzip
        fi
        '''
      }
    }

<<<<<<< HEAD
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
=======
    stage('Install Terraform (no sudo)') {
      steps {
        sh '''
        cd /tmp

        if ! command -v terraform >/dev/null; then
          wget https://releases.hashicorp.com/terraform/1.6.0/terraform_1.6.0_linux_amd64.zip -O terraform.zip
          unzip terraform.zip
          sudo mv terraform /usr/local/bin/
>>>>>>> fa6f9a0 (Fix Jenkins pipeline automation)
        fi

        terraform -version
        '''
      }
    }

    stage('Terraform Init') {
      steps {
        sh '''
        cd terraform
        terraform init
        '''
      }
    }

    stage('Terraform Validate') {
      steps {
        sh '''
        cd terraform
        terraform validate
        '''
      }
    }

    stage('Terraform Plan') {
      steps {
        sh '''
        cd terraform
<<<<<<< HEAD
        /tmp/terraform plan
=======
        terraform plan
>>>>>>> fa6f9a0 (Fix Jenkins pipeline automation)
        '''
      }
    }

    stage('Terraform Apply') {
      steps {
        sh '''
        cd terraform
<<<<<<< HEAD
        /tmp/terraform apply -auto-approve
=======
        terraform apply -auto-approve
>>>>>>> fa6f9a0 (Fix Jenkins pipeline automation)
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
