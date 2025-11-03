pipeline {
    agent any

    environment {
        TF_VERSION = "1.6.0"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Terraform') {
            steps {
                sh '''
                if ! command -v terraform &> /dev/null; then
                  wget https://releases.hashicorp.com/terraform/${TF_VERSION}/terraform_${TF_VERSION}_linux_amd64.zip
                  unzip terraform_${TF_VERSION}_linux_amd64.zip
                  sudo mv terraform /usr/local/bin/
                fi
                terraform version
                '''
            }
        }

        stage('Terraform Init') {
            steps {
                sh "terraform init"
            }
        }

        stage('Terraform Validate') {
            steps {
                sh "terraform validate"
            }
        }

        stage('Terraform Plan') {
            steps {
                sh "terraform plan -out=plan.out"
            }
        }

        stage('Terraform Apply') {
            when {
                branch 'main'
            }
            steps {
                input message: "Approve Terraform Apply?"
                sh "terraform apply -auto-approve plan.out"
            }
        }
    }
}
