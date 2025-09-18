# Setup S3 Backend for Terraform

This guide will walk you through setting up an S3 backend for Terraform, including creating an IAM user with the necessary permissions.

## 1. Sign in to AWS Console

- Go to [AWS Console](https://console.aws.amazon.com/) and log in with an account that has IAM privileges.

## 2. Navigate to IAM

- In the AWS Console, search for **IAM** and select it.

## 3. Create a New User

- Click **Users** in the left sidebar.
- Click **Create user**.

### Step 1: Specific user details

- **User name:** `terraform-<project-name>`
  - Replace `<project-name>` with the name of your project (e.g., `tax-report-api`).
- Click **Next**.

### Step 2: Set permissions

- Choose **Attach policies directly**.
- Search for and select:
  - `AmazonS3FullAccess`
  - `AmazonDynamoDBFullAccess`
  - (Add or remove policies as needed for your project)
- Click **Next**

### Step 3: Review an create

- Click **Create user**.

## 4. Generate Access Keys

- Click **Users** in the left sidebar.
- Select the user you just created.
- Go to the **Security credentials** tab.

### Create access key

- Click **Create access key**.
- For "Use case", select **Command line interface (CLI)**.
- (Optional) Add a description, e.g., `Terraform access key for <project-name>`.
- Click **Create access key**.

### Retrieve access key

- Copy the **Access key** and **Secret access key**.
- Store these credentials as environment variables in your **.zshrc**, **.bashrc**, **etc** files
- You will needs these keys to run the Terraform's scripts

```bash
export AWS_ACCESS_KEY_ID=your-access-key-id
export AWS_SECRET_ACCESS_KEY=your-secret-access-key
export AWS_DEFAULT_REGION=us-east-1
```

---

**Resource naming:**

- IAM User: `terraform-<project-name>`
  - Replace `<project-name>` with your actual project name.
