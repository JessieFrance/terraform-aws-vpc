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
```

## Requirements

| Name | Version |
|------|---------|
| terraform | >= 1.0.0 |
| aws | >= 5.0.0 |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| project_name | The name of the project to prefix resources | `string` | n/a | **yes** |
| environment | Application environment (e.g., dev, prod) | `string` | `"dev"` | no |
| vpc_cidr | The CIDR block for the VPC | `string` | `"10.0.0.0/16"` | no |
| public_subnet_cidrs | CIDR blocks for public subnets (Must match AZ count) | `list(string)` | `["10.0.1.0/24", "10.0.2.0/24"]` | no |
| private_subnet_cidrs | CIDR blocks for private subnets (Must match AZ count) | `list(string)` | `["10.0.10.0/24", "10.0.11.0/24"]` | no |
| availability_zones | List of AWS availability zones to deploy into | `list(string)` | `["us-east-1a", "us-east-1b"]` | no |

## Outputs

| Name | Description |
|------|-------------|
| vpc_id | The ID of the newly created VPC |
| public_subnet_ids | A list of the created public subnet IDs |
| private_subnet_ids | A list of the created private subnet IDs |

## License

MIT License. See LICENSE for full details.