# terraform-onlinebaqqala

Terraform IaC for [onlinebaqqala.store](https://aws.onlinebaqqala.store) —
production AWS infrastructure in eu-west-1.

For architecture documentation, security proofs, and diagrams see:
[aws-cloud-security-project](https://github.com/uerruk/aws-cloud-security-project)

## State backend

Remote state in S3 with DynamoDB locking.

| Resource | Name |
|---|---|
| S3 bucket | `onlinebaqqala-terraform-state` |
| DynamoDB table | `onlinebaqqala-terraform-locks` |
| Region | `eu-west-1` |

## Usage

```bash
terraform init
terraform plan
terraform apply
```

## What this provisions

| File | What it creates |
|---|---|
| `vpc.tf` | VPC 10.0.0.0/16, 3 public + 3 private-app + 2 DB subnets, IGW, NAT, route tables |
| `nacl.tf` | 3 NACLs — public, private-app, database tier |
| `security_groups.tf` | ALB SG, EC2 SG, RDS SG — chained by identity |
| `flow_logs.tf` | VPC Flow Logs → CloudWatch Logs |
| `alb.tf` | Internet-facing ALB, HTTPS:443 listener, HTTP→HTTPS redirect |
| `acm.tf` | ACM certificate for aws.onlinebaqqala.store |
| `ec2.tf` | Launch Template with IMDSv2 enforced |
| `asg.tf` | Auto Scaling Group, min 1 / desired 1 / max 3 |
| `rds.tf` | MySQL 8.4 primary + read replica, encrypted, deletion protection |
| `s3.tf` | Frontend bucket (public) + videos bucket (private), SSE-KMS |
| `ebs_snapshots.tf` | EBS snapshot lifecycle policy |
| `cloudfront.tf` | CloudFront distribution → ALB + S3 |
| `waf.tf` | WAF WebACL with managed rule groups |
| `kms.tf` | Customer managed key (CMK) for all encryption |
| `iam.tf` | EC2 instance role, GitHub Actions OIDC role, least-privilege policies |
| `secrets.tf` | Secrets Manager — RDS credentials encrypted with CMK |
| `guardduty.tf` | GuardDuty detector |
| `security_hub.tf` | Security Hub + CIS and AWS FSBP standards |
| `config.tf` | AWS Config recorder + 7 compliance rules |
| `cloudtrail.tf` | Multi-region CloudTrail, KMS encrypted |
| `cloudwatch.tf` | Dashboard, 5 security alarms, 4 infra alarms, SNS topics |
| `lambda.tf` | order-notification Lambda (SES email on order) |
| `provider.tf` | AWS provider, S3 backend, default tags |
| `variables.tf` | All input variables |
| `outputs.tf` | Output values |
