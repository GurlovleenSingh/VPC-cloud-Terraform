# VPC-cloud-Terraform
Using terraform to create a VPC on Amazon
Description:
**************************************
we are going to use terraform to create VPC on amazon cloud.
Creating a Virtual Private Cloud (VPC) on Amazon Web Services (AWS) using Terraform involves defining the VPC configuration in a Terraform script, which is then used to provision and manage the infrastructure on AWS. 
Step 1: Install Terraform
First, make sure you have Terraform installed on your local machine. You can download it from Terraform's official website and follow the installation instructions for your operating system.

Step 2: Configure AWS Credentials
Ensure that your AWS credentials are set up either through environment variables (AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY) or by using AWS CLI aws configure command.

Step 3: Create a Terraform Configuration File (e.g., main.tf)
Create a file named main.tf (or any other .tf file) and define your VPC configuration using Terraform's AWS provider.
CODE:
# main.tf

provider "aws" {
  region = "us-east-1"  # Set your desired AWS region
}

# Create a VPC
resource "aws_vpc" "my_vpc" {
  cidr_block = "10.0.0.0/16"
  enable_dns_support = true
  enable_dns_hostnames = true

  tags = {
    Name = "MyVPC"
  }
}

# Create an Internet Gateway
resource "aws_internet_gateway" "my_igw" {
  vpc_id = aws_vpc.my_vpc.id

  tags = {
    Name = "MyIGW"
  }
}

# Create a Route Table
resource "aws_route_table" "my_route_table" {
  vpc_id = aws_vpc.my_vpc.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.my_igw.id
  }

  tags = {
    Name = "MyRouteTable"
  }
}

# Associate Route Table with Subnet (optional, if you plan to create subnets)
# resource "aws_route_table_association" "subnet_association" {
#   subnet_id      = aws_subnet.my_subnet.id
#   route_table_id = aws_route_table.my_route_table.id
# }
Step 4: Initialize and Apply Terraform Configuration
Once you have defined your main.tf file:
Open a terminal or command prompt.
Navigate to the directory containing your .tf files.
Initialize Terraform by running
terraform init
Verify the Terraform plan by running:
terraform plan
Apply the Terraform configuration to create the VPC on AWS:
terraform apply
Step 5: Verify the VPC Creation
After Terraform applies the configuration, it will output the details of the resources created, including the VPC ID, subnet IDs (if created), route tables, etc.




