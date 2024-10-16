# Mario-game_eks_project
Super Mario Deployment Automation on Kubernetes with Terraform........
Prerequisite
You need an aws account and you need to know some basics of aws to be able to complete this project.
You will configure Terraform inside an ec2 instance.
You also need an IAM ec2 role that provide necessary permission to ec2.
Completion steps
Login and basics setup.
Setup Docker ,Terraform ,aws cli , and Kubectl.
IAM Role for EC2.
Attach IAM role with your EC2.
Building Infrastructure Using terraform.
Creation of deployment and service for EKS
Destroy all the Insrastructure
Step 1 → Login and basics setup
Login into your aws account as a root user.

Launch an EC2 instance with the following setting

Click on launch Instance

Click on connect and you are connected with your machine

Run the following commands

sudo su
apt update -y
Step 2 → Setup Docker ,Terraform ,aws cli , and Kubectl
Run the following command to Setup Docker

apt install docker.io
which docker
Add ubuntu user to the docker group

usermod -aG docker $USER # Replace with your username e.g ‘ubuntu’
Switch to Docker Group in Current Session

newgrp docker
Setup Terraform
Install wget

sudo apt install wget -y
Download the GPG key and store it correctly

wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
Add the repository to the sources list using correct quotes

echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
Update the package list

sudo apt update
Install Terraform

sudo apt install terraform -y
which terraform
AWSCLI Overview
AWS Command Line Interface (CLI) is a unified tool provided by Amazon Web Services (AWS) for managing AWS services from the command line. It allows users to interact with various AWS services and resources directly through commands, rather than through the AWS Management Console web interface.

The AWS CLI provides commands for a wide range of AWS services, including but not limited to:

Compute Services: Amazon EC2 (Elastic Compute Cloud), AWS Lambda, Amazon ECS (Elastic Container Service).
Storage Services: Amazon S3 (Simple Storage Service), Amazon EBS (Elastic Block Store), Amazon Glacier.
Database Services: Amazon RDS (Relational Database Service), Amazon DynamoDB, Amazon Redshift.
Networking Services: Amazon VPC (Virtual Private Cloud), Amazon Route 53 (Domain Name System), Amazon CloudFront (Content Delivery Network).
Security Services: AWS IAM (Identity and Access Management), AWS KMS (Key Management Service), AWS Certificate Manager.
Monitoring and Management Services: Amazon CloudWatch, AWS Config, AWS CloudFormation.
Machine Learning and Analytics Services: Amazon SageMaker, Amazon Athena, Amazon EMR (Elastic MapReduce).
Using the AWS CLI, users can perform a variety of tasks such as creating and managing AWS resources, configuring security settings, monitoring resource usage, and automating workflows. The AWS CLI commands are structured in a hierarchical manner, organized by services and subcommands.

Setup AWSCLI
Download the AWS CLI version 2 installation file
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
Install the unzip package if it’s not already installed
sudo apt-get install unzip -y
Unzip the downloaded file
unzip awscliv2.zip
Run the AWS CLI install script
sudo ./aws/install
Verify the installation of AWS CLI by checking its version
aws --version
Setup kubectl
Install curl if it’s not already installed
sudo apt install curl -y
Download the latest stable version of kubectl
curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
Install kubectl to /usr/local/bin with the appropriate permissions
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
To verify that kubectl is installed correctly, check its version
kubectl version --client
To check the path where kubectl is installed
which kubectl
At this point, every thing is setup and installed so the next step is to create an IAM EC2 Role.

Step 3 → IAM Role for EC2
The IAM role for EC2 is essential because it provides the necessary permissions for your EC2 instance to perform various tasks, such as creating an EKS cluster and managing S3 buckets. By assigning this IAM role to your EC2 instance, it grants the necessary authentication for your EC2 to make changes within your AWS account

on the search bar type IAM.
click on Roles on the left side.
click on create role and choose EC2 from the dropdown.
click on next.
choose administrator access on permission sections.
click on next and give a name to your role.
click on create role option and your IAM role is created.
Step 4 →Attach IAM role with your EC2
go to EC2 section.
click on actions → security → modify IAM role option.
choose the role from dropdown and click on update IAM role
Step 5 → Building Infrastructure Using terraform
Clone the github repo by running the following commands ---->

Create a directory named super_mario
mkdir super_mario
Change to the super_mario directory
cd super_mario
Clone the Git repository into the super_mario directory
git clone https://github.com/cloudsolutionstech/mario_project.git
Change to the mario_project directory
cd mario_project
Change to the kubectl-terraform directory
cd kubectl-terraform
Edit the backend.tf file by → vi backend.tf Note:- make sure to provide your bucket and region name in this file otherwise it doesn’t work and IAM role is also associated with your ec2 which helps ec2 to use other services such S3 bucket.

Now run the following commands
terraform init
When you execute 'terraform init,' it establishes your workspace, retrieves essential plugins, and verifies all prerequisites to enable seamless utilization of Terraform for infrastructure provisioning, updating, or management. Think of it as preparing your toolkit and gathering materials before embarking on crafting something remarkable using your computer.

terraform validate
Terraform validate examines our code, identifying any syntax errors or mistakes. It provides a success output only when the file contains no errors.

terraform plan
Terraform plan allows us to preview the modifications to our infrastructure. This command enables us to review and verify the proposed changes before granting final approval for building or modifying our application's infrastructure. It serves as the blueprint for our construction project, offering insight before making any actual alterations with Terraform.

terraform apply


terraform apply --auto-approve
Executing 'terraform apply --auto-approve' is akin to instructing the computer to proceed with building everything precisely as outlined, without prompting for confirmation at every step. This approach automates the deployment of our infrastructure, eliminating the need for constant manual input. Upon execution, Terraform analyzes our code, identifies necessary creations or alterations, and proceeds with construction, bypassing the usual approval check with the user. The apply takes 10 to 15 min for completion.

Below command is used to update the configuration of EKS

aws eks update-kubeconfig --name EKS_CLOUD --region ap-south-1
Using the command 'aws eks update-kubeconfig --name EKS_CLOUD --region ap-south-1' is essentially informing our computer that we're utilizing Amazon EKS (Elastic Kubernetes Service) in the 'us-east-1' region and wish to establish a connection to it. You can specify your preferred region accordingly.

Step 6 → Creation of deployment and service for EKS
change the directory where deployment and service files are stored. File name is "deployment.yaml" run the next command

cd ..
create the deployment

kubectl apply -f deployment.yaml
The deployment.yaml file serves as a comprehensive set of instructions for a computer system, outlining how to effectively run and manage a specific application. It furnishes essential details required for deploying and overseeing the application's lifecycle, encompassing its description, desired instance count, and configuration settings crucial for maintaining seamless operation.

Now create the service.

kubectl apply -f service.yaml
The service.yaml file functions as a structured set of rules facilitating communication among components within a software application. Resembling a directory, it dictates the means for locating and interacting with various segments of our application. This document defines the communication pathways between different parts of the application, as well as outlining accessibility for external services or users.

Run this command

kubectl get all
Run the follwing command to get the load balancer ingress. This command tells all the details of your application.

kubectl describe service mario-service
NOTE:- copy the load balancer ingress and paste it on browser and your game is running.

Before you indulge in playing and enjoying your AWS resources, remember to clean up to avoid unnecessary costs and maintain the security of your AWS account.

Load Balancer Ingress: Load Balancer Ingress is a crucial mechanism for distributing incoming internet traffic across multiple servers or services. It acts as a digital receptionist at a bustling office building entrance, directing visitors to various floors or departments to prevent overcrowding. Similarly, in the digital realm, Load Balancer Ingress ensures a seamless user experience, enhances application performance, and prevents any single server from being overwhelmed by excessive traffic.

Step 7 → Destroy all the Insrastructure
Below commands delete your deployment and service.

kubectl delete service mario-service
kubectl delete deployment mario-deployment
After service and deployment destruction let’s destroy the infrastructure by the following command it will delete your EKS cluster.

cd kubectl-terraform
terraform destroy --auto-approve
This might take up to 5 - 10 mins before everything is destroyed. 03. After everything is destroyed through terraform, you can now go to your EC2 and terminate your Instance.....
