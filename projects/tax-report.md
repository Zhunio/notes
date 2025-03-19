# Tax Report

## Overview

This project involves migrating the existing infrastructure to AWS services, including changing email credentials, migrating file storage to an AWS S3 Bucket, and migrating the MySQL database to AWS Aurora. The tasks are organized into sections with checkboxes for tracking progress.

## Table of Contents

- [Create lambda function with API Gateway using Terraform](#create-lambda-function-with-api-gateway-using-terraform)
- [Migrate file storage to AWS S3 Bucket](#migrate-file-storage-to-aws-s3-bucket)
- [Migrate MySQL to AWS Aurora](#migrate-mysql-to-aws-aurora)
  - [Create AWS Aurora instance using Terraform](#create-aws-aurora-instance-using-terraform)
  - [Migrate MySQL database to AWS Aurora](#migrate-mysql-database-to-aws-aurora)
- [Change email credentials because zhunio-app.com no longer works](#change-email-credentials-because-zhunio-appcom-no-longer-works)

## Create lambda function with API Gateway using Terraform

- [-] Create simple lambda function with API Gateway using Terraform
- [-] Update CI/CD pipeline to deploy lambda function with API Gateway on merge to main

## Migrate file storage to AWS S3 Bucket

- [-] Create AWS S3 Bucket using Terraform
- [-] Migrate files to AWS S3 Bucket
- [-] Update application configuration to connect to AWS S3 Bucket
- [-] Update documentation

## Migrate MySQL to AWS Aurora

### Create AWS Aurora instance using Terraform

- [-] Create AWS Aurora instance using Terraform
- [-] Update application configuration to connect to AWS Aurora instance
- [-] Update CI/CD pipeline to run migration scripts on AWS Aurora instance on merge to main

### Migrate MySQL database to AWS Aurora

- [-] Run migration scripts on AWS Aurora instance
- [-] Migrate data from old MySQL database to AWS Aurora instance

## Change email credentials because zhunio-app.com no longer works

- [-] Update email service configuration with new credentials
- [-] Update documentation
