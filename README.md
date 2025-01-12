# AWS VPC Terraform Module

This Terraform module creates a VPC with public and private subnets across multiple availability zones in AWS.

## Features

- Creates a VPC with customizable CIDR block
- Creates public and private subnets across multiple availability zones
- Optional NAT Gateway deployment (single or multiple)
- Configurable DNS settings
- Flexible tagging system
- VPC Flow Logs (optional)

## Prerequisites

- Terraform >= 0.13.x
- AWS Provider
- AWS credentials configured

## Usage

```hcl
module "my_vpc" {
  source = "./vpc-module"

  vpc_name            = "my-vpc"
  vpc_cidr           = "10.0.0.0/16"
  availability_zones = ["us-west-2a", "us-west-2b", "us-west-2c"]
  
  private_subnet_cidrs = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  public_subnet_cidrs  = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]
  
  enable_nat_gateway = true
  single_nat_gateway = true
  
  environment = "development"
  
  tags = {
    Project     = "MyProject"
    Owner       = "DevOps Team"
  }
}


Module Input Variables
Name	Description	Type	Default	Required
vpc_name	Name of the VPC	string	-	yes
vpc_cidr	CIDR block for VPC	string	-	yes
availability_zones	List of availability zones	list(string)	-	yes
private_subnet_cidrs	CIDR blocks for private subnets	list(string)	no
public_subnet_cidrs	CIDR blocks for public subnets	list(string)	no
enable_nat_gateway	Enable NAT Gateway for private subnets	bool	false	no
single_nat_gateway	Use single NAT Gateway for all private subnets	bool	false	no
enable_dns_hostnames	Enable DNS hostnames in the VPC	bool	true	no
enable_dns_support	Enable DNS support in the VPC	bool	true	no
environment	Environment name	string	-	yes
tags	Additional tags for resources	map(string)	{}	no
Module Outputs
Name	Description
vpc_id	The ID of the VPC
private_subnet_ids	List of private subnet IDs
public_subnet_ids	List of public subnet IDs
nat_gateway_ids	List of NAT Gateway IDs
vpc_cidr_block	The CIDR block of the VPC

Examples
Basic VPC with public subnets only

module "vpc_basic" {
  source = "./vpc-module"

  vpc_name            = "basic-vpc"
  vpc_cidr           = "172.16.0.0/16"
  availability_zones = ["us-east-1a", "us-east-1b"]
  public_subnet_cidrs = ["172.16.1.0/24", "172.16.2.0/24"]
  
  environment = "staging"
}

#VPC with both public and private subnets and NAT Gateway

module "vpc_complete" {
  source = "./vpc-module"

  vpc_name            = "complete-vpc"
  vpc_cidr           = "10.0.0.0/16"
  availability_zones = ["us-west-2a", "us-west-2b"]
  
  private_subnet_cidrs = ["10.0.1.0/24", "10.0.2.0/24"]
  public_subnet_cidrs  = ["10.0.101.0/24", "10.0.102.0/24"]
  
  enable_nat_gateway = true
  single_nat_gateway = true
  
  environment = "production"
  
  tags = {
    Project     = "ProductionApp"
    CostCenter  = "12345"
  }
}

Resource Creation
This module creates the following resources:

VPC

Internet Gateway

Public and Private Subnets (as specified)

Route Tables

NAT Gateways (if enabled)

Route Table Associations

Network ACLs

Best Practices
CIDR Block Planning:

Use non-overlapping CIDR blocks

Plan for future growth when allocating subnet sizes

Availability Zones:

Use multiple AZs for high availability

Balance the number of public and private subnets

NAT Gateways:

Use single NAT Gateway for cost optimization in non-production

Use multiple NAT Gateways for high availability in production

Tagging:

Always include environment tags

Follow your organization's tagging strategy

Common Issues and Troubleshooting
CIDR Block Conflicts:

Ensure VPC CIDR doesn't overlap with other VPCs

Verify subnet CIDR blocks are within VPC CIDR range

NAT Gateway Issues:

Check Internet Gateway attachment

Verify route table configurations

Subnet Association:

Confirm correct route table associations

Verify CIDR block calculations

Contributing
Please feel free to submit issues, fork the repository, and create pull requests for any improvements.

License
This module is released under the MIT License.


This README provides:

1. Clear module description and features
2. Detailed usage instructions
3. Complete documentation of inputs and outputs
4. Multiple usage examples
5. Best practices and troubleshooting tips
6. Resource creation details
7. Common issues and their solutions

Users can easily understand:

- What the module does
- How to use it
- What resources it creates
- How to troubleshoot common issues
- Best practices for implementation

The examples section provides different use cases, making it easier for users to adapt the module to their needs.
