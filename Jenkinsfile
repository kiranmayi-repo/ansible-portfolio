pipeline {
    agent any

    environment {
        TF_VERSION = "1.6.0"
        PATH = "$PATH:/usr/local/bin"
    }

// --- Terraform + Ansible pipeline snippet (paste into your Jenkinsfile) ---
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

    stage('Prepare agent') {
      steps {
        sh '''
        set -euo pipefail
        # ensure unzip is available
        if ! command -v unzip >/dev/null 2>&1; then
          echo "Installing unzip"
          sudo apt-get update -y
          sudo apt-get install -y unzip
        fi
        '''
      }
    }

    stage('Install Terraform (no sudo)') {
      steps {
        sh '''
        set -euo pipefail

        # cleanup any previous files to avoid interactive prompt
        rm -f terraform terraform_*.zip || true

        echo "Downloading Terraform..."
        wget -q https://releases.hashicorp.com/terraform/1.6.0/terraform_1.6.0_linux_amd64.zip -O terraform_1.6.0_linux_amd64.zip

        echo "Unzipping Terraform (force overwrite)..."
        unzip -o terraform_1.6.0_linux_amd64.zip

        chmod +x terraform

        # move to /tmp so no sudo required, put into PATH for this build
        mv -f terraform /tmp/

        echo "Terraform installed to /tmp"
        /tmp/terraform --version
        '''
      }
    }

    stage('Terraform Init') {
      steps {
        sh '''
        set -euo pipefail
        /tmp/terraform init || true
        '''
      }
    }

    stage('Terraform Validate') {
      steps {
        sh '''
        set -euo pipefail
        /tmp/terraform validate || true
        '''
      }
    }

    stage('Terraform Plan') {
      steps {
        sh '''
        set -euo pipefail
        /tmp/terraform plan -out=tfplan || true
        '''
      }
    }

    stage('Terraform Apply') {
      steps {
        sh '''
        set -euo pipefail
        # apply only if plan exists - adjust for your flow
        if [ -f tfplan ]; then
          /tmp/terraform apply -auto-approve tfplan || true
        else
          echo "No tfplan file found — skipping apply"
        fi
        '''
      }
    }

    // Optionally add Ansible stages after Terraform
    stage('Run Ansible Playbook (if required)') {
      steps {
        sh '''
        set -euo pipefail
        # ensure ansible is available on agent
        if ! command -v ansible-playbook >/dev/null 2>&1; then
          sudo apt-get update -y
          sudo apt-get install -y ansible
        fi
        # run your playbook (adjust path if needed)
        ansible-playbook -i inventory/dev playbooks/nginx-deployment.yml || true
        '''
      }
    }
  } // stages
} // pipeline
// --- end snippet ---

    parameters {
        booleanParam(name: 'APPLY', defaultValue: false, description: 'Apply Terraform changes?')
    }
}

