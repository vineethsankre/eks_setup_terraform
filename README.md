# eks_setup_terraform
creating EKS cluster using Terraform 

# Run the Project
1. Initialize Terraform

terraform init

2. Apply the Infrastructure

terraform apply -auto-approve

🔧 Configure kubectl

aws eks update-kubeconfig --name eks-default-vpc --region us-east-1

kubectl get nodes
