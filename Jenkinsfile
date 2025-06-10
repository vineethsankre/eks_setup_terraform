
pipeline {
    agent any

    environment {
        // Define environment variables
        AWS_ACCESS_KEY_ID = credentials('aws-access-key') // AWS credentials (if needed)
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key') // AWS credentials (if needed)
    }

    stages {
        stage('CLONE SCM') {
            steps {
                echo 'Cloning code from GitHub...'
                git branch: 'main', url: 'https://github.com/vineethsankre/eks_setup_terraform.git'
            }
        }

       stage('Install Terraform') {
    steps {
        script {
            sh '''
                # Set Terraform version
                TERRAFORM_VERSION="1.6.6"

                # Install unzip if not installed
                if ! command -v unzip &> /dev/null; then
                    echo "Installing unzip..."
                    sudo apt-get update && sudo apt-get install -y unzip
                fi

                # Check if Terraform is already installed
                if ! command -v terraform &> /dev/null; then
                    echo "Installing Terraform..."
                    curl -LO https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip
                    unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip
                    sudo mv terraform /usr/local/bin/
                    rm terraform_${TERRAFORM_VERSION}_linux_amd64.zip
                else
                    echo "Terraform is already installed."
                fi

                # Verify installation
                terraform --version
            '''
        }
    }
}


        stage('Terraform Init') {
            steps {
                // Initialize Terraform
                echo 'Initializing Terraform...'
            sh '''                        
                        terraform init
                    '''
                
            }
        }

        stage('Terraform Plan') {
            steps {
                // Run Terraform plan
                echo 'Running Terraform Plan...'
                 sh '''                
                        terraform plan -out=tfplan
                    '''
            }
        }

        stage('Terraform Apply') {
            steps {
                // Apply Terraform changes
                echo 'Applying Terraform changes...'
                 sh '''
                      terraform destroy -auto-approve
                    '''
            }
        }
    }

    post {
        success {
            echo 'Terraform deployment completed successfully!'
        }
        failure {
            echo 'Terraform deployment failed. Check the logs for details.'
        }
        always {
            // Clean up workspace
            cleanWs()
        }
    }
}
