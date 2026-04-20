# Terraform setup

## Prerequisites

- Terraform >= 1.6.0
- AWS CLI configured with appropriate credentials
- Access to the S3 state bucket and DynamoDB lock table

## First time setup

```bash
terraform init
```

This connects to the remote state backend in S3.

## Plan before applying

```bash
terraform plan
```

Always review the plan output before applying.

## Apply

```bash
terraform apply
```

Type `yes` when prompted.

## State

State is stored remotely — never commit `.tfstate` files.
The DynamoDB table prevents concurrent applies from corrupting state.
