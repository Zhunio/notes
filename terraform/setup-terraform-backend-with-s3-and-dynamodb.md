# Setup Terraform Backend with AWS S3 and DynamoDB

This guide will walk you through setting up a Terraform backend using AWS S3 and DynamoDB for state management and locking.

## Prequisites

- [Setup IAM User for Terraform](setup-terraform-iam-user.md)

## Create infrastructure using Terraform

#### 1. Create the following file structure:

```sh
sample-project
├─ infrastructure        # code for infrastructure
│  └─ backend            # code to create backend
│     └─ main.tf
└─ ... other files
```

#### 2. Create S3 bucket and DynamoDB table for terraform backend

```terraform
# infrastructure/backend/main.tf

# 💀 1. Set the terraform project name
variable "project_name" {
  description = "Name of the project"
  type        = string
  default     = "tax-report"
}

locals {
  # Name of the S3 bucket to use for storing the terraform backend
  backend_bucket_name = "terraform-${var.project_name}-backend-bucket"
  # Name of the DynamoDB table to use for locking the terraform state
  backend_dynamodb_table_name = "terraform-${var.project_name}-backend-dynamodb"
}

provider "aws" {
  # AWS region to create resources in
  region = "us-east-1"
}

# Create S3 bucket for storing the terraform backend
resource "aws_s3_bucket" "backend_bucket" {
  bucket = local.backend_bucket_name
}

# Enable versioning to keep multiple variants of an object in the S3 bucket
resource "aws_s3_bucket_versioning" "backend_bucket_versioning" {
  bucket = aws_s3_bucket.backend_bucket.id

  versioning_configuration {
    status = "Enabled"
  }
}

# Enable server-side encryption by default for all objects in the S3 bucket
resource "aws_s3_bucket_server_side_encryption_configuration" "backend_bucket_encryption" {
  bucket = aws_s3_bucket.backend_bucket.id

  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}

# Create DynamoDB table for locking the terraform state
resource "aws_dynamodb_table" "backend_dynamodb" {
  name = local.backend_dynamodb_table_name

  # Use on-demand billing (no need to specify read/write capacity)
  billing_mode = "PAY_PER_REQUEST"

  # Primary key for the table
  hash_key = "LockID"

  # Define the primary key attribute
  attribute {
    name = "LockID"
    type = "S" # String type
  }
}
```

#### 3. Execute backend terraform script

```sh
cd infrastructure/backend       # navigate to infrastructure/backend
terraform apply -auto-aprove    # approve
```

## Destroy infrastructure

```sh
cd infrastructure/backend       # navigate to infrastructure/backend
terraform destroy -auto-aprove  # approve
```

## Resource naming

- Project Name: `<project-name>`
- Terraform Backend Resources:
  - IAM User: `terraform-<project-name>`
    - Policies:
      - `AmazonS3FullAccess`
      - `AmazonDynamoDBFullAccess`
  - S3 Bucket: `terraform-<project-name>-backend-bucket`
  - DynamoDB Table: `terraform-<project-name>-backend-dynamodb`
