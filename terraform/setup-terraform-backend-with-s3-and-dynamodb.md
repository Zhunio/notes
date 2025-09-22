# Setup Terraform Backend with AWS S3 and DynamoDB

This guide will walk you through setting up a Terraform backend using AWS S3 and DynamoDB for state management and locking.

## Overview

- [Create New IAM User](#create-new-iam-user)
- [Create infrastructure using Terraform](#create-infrastructure-using-terraform)
- [Configure Terraform Backend](#configure-terraform-backend)

## Create New IAM User

- Sign in to AWS Console
  - Go to [AWS Console](https://console.aws.amazon.com/) and log in with an account that has IAM privileges
- Navigate to IAM
  - In the AWS Console, search for **IAM** and select it
  - Click **Users** in the left sidebar
  - Click **Create user**
    - _Step 1: Specific user details_
      - **User name:** `terraform-<project-name>`
      - Click **Next**
    - _Step 2: Set permissions_
      - Choose **Attach policies directly**
      - Search for and select:
        - `AmazonS3FullAccess`
        - `AmazonDynamoDBFullAccess`
        - (Add or remove policies as needed for your project)
      - Click **Next**
    - _Step 3: Review an create_
      - Click **Create user**
    - _Step 4: Generate Access Keys_
      - Click **Users** in the left sidebar
      - Select the user you just created
      - Go to the **Security credentials** tab
    - _Step 5: Create access key_
      - Click **Create access key**
      - For "Use case", select **Command line interface (CLI)**
      - (Optional) Add a description, e.g. `Terraform access key for <project-name>`
      - Click **Create access key**
    - _Step 6: Retrieve access key_
      - Copy the **Access key** and **Secret access key**
      - Store these credentials as environment variables in your **.zshrc**, **.bashrc**, **etc** files

```sh
# .zshrc
export AWS_ACCESS_KEY_ID=your-access-key-id
export AWS_SECRET_ACCESS_KEY=your-secret-access-key
export AWS_DEFAULT_REGION=us-east-#
```

## Create infrastructure using Terraform

#### 1. Create the following file structure:

```sh
sample-project
├─ infrastructure        # code for infrastructure
│  ├─ backend            # code to create backend
│  │  └─ main.tf
│  └─ main.tf
└─ ... other files
```

#### 2. Create S3 bucket and DynamoDB table for terraform backend

```terraform
# infrastructure/backend-setup/main.tf
#
# Variable for the project name
variable "project_name" {
 description = "Name of the project for naming resources"
 type        = string
 default     = "<project-name>"
}

locals {
  # Name of the S3 bucket to use for storing the terraform backend
  backend_bucket_name = "terraform-backend-s3-${var.project_name}"
  # Name of the DynamoDB table to use for locking the terraform state
  backend_dynamodb_table_name = "terraform-backend-dynamodb-${var.project_name}"
}

# Configure the AWS provider
provider "aws" {
  # AWS region to create resources in
  region = "us-east-1"
}

# Create S3 bucket for storing the terraform backend
resource "aws_s3_bucket" "terraform_state" {
  # Name of the S3 bucket to use for storing the terraform backend
  bucket = local.backend_bucket_name
}

# Enable versioning to keep multiple variants of an object in the S3 bucket
resource "aws_s3_bucket_versioning" "terraform_state" {
  bucket = aws_s3_bucket.terraform_state.id

  versioning_configuration {
    status = "Enabled"
  }
}

# Enable server-side encryption by default for all objects in the S3 bucket
resource "aws_s3_bucket_server_side_encryption_configuration" "terraform_state" {
  bucket = aws_s3_bucket.terraform_state.id

  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}

# Create DynamoDB table for locking the terraform state
resource "aws_dynamodb_table" "terraform_locks" {
  # Name of the DynamoDB table, provided via variables.tf
  name         = local.backend_dynamodb_table_name

  # Use on-demand billing (no need to specify read/write capacity)
  billing_mode = "PAY_PER_REQUEST"

  # Primary key for the table
  hash_key     = "LockID"

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
terraform init                  # initialize terraform
terraform plan                  # review the terraform plan
terraform apply -auto-aprove    # approve
```

### Configure Terraform Backend

#### 1. After resources are created, update your main Terraform configuration to use the backend:

```terraform
# infrasture/main.tf
#
# Configure the S3 backend for Terraform state
terraform {
  # Define the S3 backend
  backend "s3" {
    # Name of the S3 bucket to store the state file
    bucket         = var.backend_bucket_name
    # Path within the bucket for the state file
    key            = "global/s3/terraform.tfstate"
    # AWS region where the bucket is located
    region         = "us-east-1"
    # DynamoDB table used for state locking and consistency
    dynamodb_table = var.backend_dynamodb_table_name
    # Ensures the state file is encrypted at rest
    encrypt        = true
  }
}
```

#### 2. Initialize Terraform to use the new backend:

```sh
terraform init
```

**Resource naming:**

- Project Name: `<project-name>`
- Terraform Backend Resources:
  - IAM User: `terraform-<project-name>`
  - S3 Bucket: `terraform-backend-s3-<project-name>`
  - DynamoDB Table: `terraform-backend-dynamodb-<project-name>`
