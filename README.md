# Multi-AZ VPC Infrastructure

This Terraform project creates a Multi-Availability Zone (AZ) VPC infrastructure on AWS, including public and private subnets, an EC2 instance, and an RDS MySQL database.

## Architecture

The infrastructure includes:
- **VPC**: 10.0.0.0/16 CIDR block
- **Public Subnet**: 10.0.1.0/24 in ap-south-1a (with internet access)
- **Private Subnets**: 10.0.2.0/24 (ap-south-1a) and 10.0.3.0/24 (ap-south-1b) for RDS
- **EC2 Instance**: t2.micro in public subnet with SSH access
- **RDS MySQL**: db.t3.micro in private subnets (Multi-AZ)
- **Security Groups**: Configured for SSH (EC2) and MySQL (RDS)
- **Internet Gateway**: For public subnet internet access

## Prerequisites

- [Terraform](https://www.terraform.io/downloads.html) installed (v1.0+)
- AWS account with appropriate permissions
- SSH key pair (public key should be in `terraform-key.pub`)

## Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/N333l/Multi-AZ-VPC-Infra.git
   cd Multi-AZ-VPC-Infra
   ```

2. **Configure AWS credentials**:
   - Create `terraform.tfvars` file with your AWS credentials:
     ```
     aws_access_key = "YOUR_ACCESS_KEY"
     aws_secret_key = "YOUR_SECRET_KEY"
     region = "ap-south-1"  # or your preferred region
     ```
   - Alternatively, use AWS CLI configuration or environment variables

3. **Generate SSH key pair** (if not already done):
   ```bash
   ssh-keygen -f terraform-key -N ""
   ```

4. **Initialize Terraform**:
   ```bash
   terraform init
   ```

5. **Plan the deployment**:
   ```bash
   terraform plan
   ```

6. **Apply the configuration**:
   ```bash
   terraform apply
   ```

## Outputs

After deployment, Terraform will output:
- `ec2_public_ip`: Public IP of the EC2 instance
- `rds_endpoint`: Endpoint URL for the RDS database

## Usage

- **Connect to EC2**: `ssh -i terraform-key ec2-user@<ec2_public_ip>`
- **Connect to RDS**: Use the endpoint with MySQL client (username: admin, password: password123)

## Cleanup

To destroy all resources:
```bash
terraform destroy
```

## Security Notes

- The RDS password is hardcoded in the configuration for demo purposes. In production, use Terraform variables or AWS Secrets Manager.
- SSH access is open to 0.0.0.0/0. Restrict this in production.
- Ensure your AWS credentials are kept secure and not committed to version control.

## Requirements

| Name | Version |
|------|---------|
| terraform | >= 1.0 |
| aws | ~> 5.0 |

## Providers

| Name | Version |
|------|---------|
| aws | ~> 5.0 |