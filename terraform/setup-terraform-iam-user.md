# Setup IAM User for Terraform

This guide will help you set up an IAM user in AWS with the necessary permissions for Terraform to manage your infrastructure.

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

## Set Environment Variables

```sh
# .zshrc
export AWS_ACCESS_KEY_ID=your-access-key-id
export AWS_SECRET_ACCESS_KEY=your-secret-access-key
export AWS_DEFAULT_REGION=us-east-#
```
