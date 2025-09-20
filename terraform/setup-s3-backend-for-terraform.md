# Setup S3 Backend for Terraform

This guide will help you set up an S3 bucket and DynamoDB table to use as a remote backend for Terraform state management.

## 1. Create New IAM User

- 1. Sign in to AWS Console
  - Go to [AWS Console](https://console.aws.amazon.com/) and log in with an account that has IAM privileges
- 2. Navigate to IAM
  - In the AWS Console, search for **IAM** and select it
  - Click **Users** in the left sidebar
  - Click **Create user**
    - **Step 1: Specific user details**
      - **User name:** `terraform-<project-name>` (e.g. `terraform-tax-report`)
      - Click **Next**
    - **Step 2: Set permissions**
      - Choose **Attach policies directly**
      - Search for and select:
        - `AmazonS3FullAccess`
        - `AmazonDynamoDBFullAccess`
        - (Add or remove policies as needed for your project)
      - Click **Next**
    - **Step 3: Review an create**
      - Click **Create user**
    - **Step 4: Generate Access Keys**
      - Click **Users** in the left sidebar
      - Select the user you just created
      - Go to the **Security credentials** tab
    - **Step 5: Create access key**
      - Click **Create access key**
      - For "Use case", select **Command line interface (CLI)**
      - (Optional) Add a description, e.g. `Terraform access key for <project-name>`
      - Click **Create access key**
    - **Step 6: Retrieve access key**
      - Copy the **Access key** and **Secret access key**
      - Store these credentials as environment variables in your **.zshrc**, **.bashrc**, **etc** files

```sh
#  .zshrc
export AWS_ACCESS_KEY_ID=your-access-key-id
export AWS_SECRET_ACCESS_KEY=your-secret-access-key
export AWS_DEFAULT_REGION=us-east-#
```

## 2. Create Terraform script to crate S3 Bucket and DynamoDB Table for Terraform Backend

_I. Create the following files:_

```sh
terraform-backend-setup/
├──  main.tf
├──  variables.tf
```

_II. Define Terraform Variables for Terraform Backend_

```terraform
#  variables.tf
variable "project_name" {
 description = "Name of the project for naming resources"
 type        = string
 default     = "<project-name>" # e.g. "tax-report"
}

variable "backend_bucket_name" {
 description = "Name of the S3 bucket to use for the Terraform backend"
 type        = string
}

variable "backend_dynamodb_table_name" {
 description = "Name of the DynamoDB table to use for state locking in the terraform backend"
 type        = string
}

locals {
  backend_bucket_name = "terraform-backend-s3-${var.project_name}"
  backend_dynamodb_table_name = "terraform-backend-dynamodb-${var.project_name}"
}
```

_III. Define the AWS Provider_

```terraform
#  main.tf
#
# Configure the AWS provider
provider "aws" {
  # AWS region to create resources in
  region = "us-east-1"
}
```

_IV. Create the S3 Bucket for Terraform State_

```terraform
#  main.tf
#
# Create S3 bucket for Terraform state
resource "aws_s3_bucket" "terraform_state" {
  # Name of the S3 bucket, provided via variables.tf
  bucket = var.backend_bucket_name

  # Enable versioning to keep multiple variants of an object in the bucket
  versioning {
    enabled = true
  }

  # Enable server-side encryption by default for all objects in the bucket
  server_side_encryption_configuration {
    rule {
      apply_server_side_encryption_by_default {
        sse_algorithm = "AES256"
      }
    }
  }
}
```

_V. Create the DynamoDB Table_

```terraform
#  main.tfstate
#
# Create DynamoDB table for Terraform state locking
resource "aws_dynamodb_table" "terraform_locks" {
  # Name of the DynamoDB table, provided via variables.tf
  name         = var.backend_dynamodb_table_name

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

_VI. Initialize Terraform and Apply_

```sh
terraform init
terraform apply # review and approve the plan
```

_VII. Configure Terraform Backend_

After resources are created, update your main Terraform configuration to use the backend:

```terraform
#  main.tf
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

---

## 6. Re-initialize Terraform

```sh
terraform init
```

This will migrate your state to the S3 backend with DynamoDB locking.

---

**Summary:**

- Write Terraform to create S3 bucket and DynamoDB table
- `terraform apply` to provision
- Configure backend block
- `terraform init` to use remote backend

Let me know if you need a full example file or further details!

---

**Resource naming:**

- IAM User: `terraform-<project-name>`
  - Replace `<project-name>` with your actual project name.
