# Use terraform to install and run a docker image for Nginx

Step 1--Create an AWS EC2 instance/ Ubuntu


Step 2--Install terraform in EC2 intsance with below links:-
**sudo apt-get update && sudo apt-get install -y gnupg software-properties-common**

--Install Hashicorp key GPG:-
**wget -O- https://apt.releases.hashicorp.com/gpg | \
  gpg --dearmor | \sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg**

--Verify the key fingerprint:-
**gpg --no-default-keyring \--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \--fingerprint**

--Adding Hashicorp repo in your system:-
**echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
  sudo tee /etc/apt/sources.list.d/hashicorp.list**

--Download package info from hashicorp:-
**sudo apt update**

--Install Terraform:-
**sudo apt-get install terraform**




Step 3--Create a main.tf file with the below contents:-

--Start with terraform block **provider to be used in automation** **give provider name with source and version**
**terraform {
       required_providers {
       docker = {
       source = "kreuzwerker/docker"
       version = "~> 2.21.0"
}
}
}

--Provider is a plugin that terraform uses to create and manage resources

**provider "docker" {}**

--Resource block has two strings before the block; resouce name and resource type. In this example the first resource type is docker_image and the name is Nginx

**resource "docker_image" "nginx" {
name           = "nginx:latest"
keep_locally = false
}

**resource "docker_container" "nginx" {
image = docker_image.nginx.latest
name = "nginx-tf"
ports {
       internal = 80
       external = 80
}
}



Step 4: In case if docker is not installed

        Install docker **sudo-apt-get install docker.io**
        
        List container **sudo docker ps**
        
        To set permission **sudo chown $USER /var/run/docker.sock**
        
        
        
Step 5: Run Terraform commands:-

        terraform init — **the terraform init command performs Backend Initialization, and Plugin Installation**
        terraform plan — **shows you what actions will be taken without actually performing the planned actions**
        terraform validate — **validates the configuration files in your directory and does not access any remote state or services**
        terraform apply — **Creates or updates infrastructure depending on the configuration files** **Enter "YES" to validate"
      
      
Step 6: Final Step you can now see the deployed docker image by running **localhost 8080**, if you are on your local machine

        Otherwise you can also run the EC2 **Public IP address** to see the deployed docker image 
![nginx](https://user-images.githubusercontent.com/113809732/210851157-73f58591-4ec5-4b5b-a802-89c80635746f.png)


        
