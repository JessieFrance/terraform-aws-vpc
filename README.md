# AWS Serverless-Friendly VPC Terraform Module

A Terraform module to deploy a highly available, multi-AZ Virtual Private Cloud (VPC) on AWS. This module is designed with serverless architectures and strict cost optimization in mind, making it ideal for low-traffic applications, hobby projects, or staging environments.

## Architecture Features

* **Multi-AZ Availability:** Deploys subnets across two Availability Zones for high availability.
* **Strict Network Isolation:** Creates distinct Public and Private subnets. Private subnets have no route to the internet, keeping your database layer secure.
* **Cost Optimized (No NAT Gateway):** AWS NAT Gateways cost ~$32+/month per zone just to sit idle. This module deliberately omits NAT Gateways. Serverless resources (like AWS Lambda or API Gateway) can interact with resources in this VPC via internal networking or managed credentials without requiring an active outbound NAT route.
* **DNS Supported:** Enables DNS hostnames and resolution natively within the VPC.

## Usage

You can call this module locally or reference it directly from a Git source:

```hcl
module "vpc" {
  source       = "./modules/terraform-aws-vpc" # Or your GitHub URL
  project_name = "my-awesome-app"
  environment  = "dev"

  # Optional overrides
  vpc_cidr             = "10.0.0.0/16"
  public_subnet_cidrs  = ["10.0.1.0/24", "10.0.2.0/24"]
  private_subnet_cidrs = ["10.0.10.0/24", "10.0.11.0/24"]
  availability_zones   = ["us-east-1a", "us-east-1b"]
}