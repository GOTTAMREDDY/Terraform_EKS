											Creation of EKS Cluster
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Jenkins + Terraform + EKS 
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Step 1: Launching an EC2 Instance and Installation of Required Tools
=======================================================
1.1 Launch VM - Ubuntu 22.04, t2.medium

1.2 Tools Installation
Java and Jenkins Installation Commands
---------------------------------------------------------
#!/bin/bash
sudo apt update
sudo apt install fontconfig openjdk-21-jre
java -version
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins

Terraform Installation
---------------------------------------------------------
#!/bin/bash
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
gpg --no-default-keyring \
--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
--fingerprint
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt-get install terraform -y

K8S Installation
---------------------------------------------------------
#!/bin/bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl 

AWS CLI Installation
---------------------------------------------------------
#!/bin/bash
sudo apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

Step 2: Creation of IAM User and Generation of Secret Keys
=======================================================
Create IAM User - Generate Access and Secret Keys to Configuration

Step 3: Jenkins Setup and Configuration 
=======================================================
Install Pipeline Stage View plugin
Configure the access and secret keys in Jenkins

Step 4: Writing Terraform Files 
=======================================================
<Refer the GitHub Repo for the Terraform Files>
Repo URL: https://github.com/GOTTAMREDDY/Terraform_EKS.git

Step 5: Creation of Jenkins Pipeline for Automating EKS Cluster Creation using Terraform
====================================================================
Create Pipeline Job


Before saving the job, goto General ----> 'Check' This project is parameterized ----> Choice Parameter ----> Name: action ----> Choices: 
apply
destroy (Make sure to write these in two lines) ----> Apply ----> Save ----> Build With Parameters ----> It will ask you to apply or destroy, select 'apply'  ----> Build


Once the job is built successfully, goto EKS in AWS ----> You will see the cluster ----> Open cluster ----> 'Resources' tab. Verify things ----> 'Add-ons' tab. Verify things ----> 'Compute' tab. Verify things.
This means we have successfully created the K8S Cluster

To interact with K8S Cluster;
Open VS Code Editor ----> Make sure to configure AWS Access and Secret keys ----> Execute the below command ----> 
aws eks update-kubeconfig --region us-east-1 --name <ClusterName> ----> To create a pod ----> kubectl run nginx --image=nginx ----> You can see the pod got created with the name 'nginx' ----> kubectl get pods ----> You will see the 'nginx' pod

To destroy everything created ----> Open Jenkins job ----> Click on 'Build with parameters' ----> Select 'Destroy' ----> Build

Terminate the instance also after destroying
















