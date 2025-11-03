pipeline {
    agent any

    environment {
        TF_VERSION = "1.6.0"
        PATH = "$PATH:/usr/local/bin"
    }

    stages {
        stage ('Install Terraform') {
            steps {
                sh '''
                if ! command -v terraform &> /dev/null
                then
                    echo "Terraform not found, installing..."
                    wget https://releases.hashicorp.com/terraform/${TF_VERSION}/terraform_${TF_VERSION}_linux_amd64.zip
                    unzip terraform_${TF_VERSION}_linux_amd64.zip
                    sudo mv terraform /usr/local/bin/
                else
                    echo "Terraform already installed"
                fi
                '''
            }
        }

        stage('Terraform Init') {
            steps {
                sh '''
                terraform init
                '''
            }
        }

        stage('Terraform Validate') {
            steps {
                sh '''
                terraform validate
                '''
            }
        }

        stage('Terraform Plan') {
            steps {
                sh '''
                terraform plan
                '''
            }
        }

        stage('Terraform Apply') {
            when {
                expression { return params.APPLY == true }
            }
            steps {
                sh '''
                terraform apply -auto-approve
                '''
            }
        }
    }

    parameters {
        booleanParam(name: 'APPLY', defaultValue: false, description: 'Apply Terraform changes?')
    }
}

