pipeline {
    agent any

    environment {
        TF_VERSION = "1.6.0"
        PATH = "$PATH:/usr/local/bin:/tmp"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/kiranmayi-repo/ansible-portfolio.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                set -euo pipefail

                # Install unzip if missing
                if ! command -v unzip >/dev/null 2>&1; then
                    apt-get update -y
                    apt-get install -y unzip
                fi
                '''
            }
        }

        stage('Install Terraform without sudo') {
            steps {
                sh '''
                wget https://releases.hashicorp.com/terraform/${TF_VERSION}/terraform_${TF_VERSION}_linux_amd64.zip -O terraform.zip
                unzip -o terraform.zip
                mv terraform /usr/local/bin/terraform
                terraform --version
                '''
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'terraform validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan'
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -auto-approve'
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

