# Terraform Level 1 Certification Practice Log

**Date:** 01 August 2026

**Platform:** KodeKloud
**Track:** Terraform Level 1 Certification
**Focus:** Terraform fundamentals, resource creation, resource association, dependencies, and AWS provider resources using Infrastructure as Code (IaC).

---

# Tasks Completed

1. Created an EC2 Instance
2. Attached an Elastic IP to an EC2 Instance
3. Attached an IAM Policy to an IAM User
4. Created an IAM User
5. Created a DynamoDB Table
6. Created an AWS Secrets Manager Secret
7. Created a Private S3 Bucket
8. Created a Public S3 Bucket
9. Created an IPv4 VPC
10. Created an IPv6-enabled VPC

---

# Task 1 — Create EC2 Instance

## Objective

Provision an EC2 instance using Terraform with:

* Amazon Linux AMI
* t2.micro
* RSA Key Pair
* Default Security Group

## Terraform Resources Used

```hcl
aws_instance
aws_key_pair
tls_private_key
local_file
data "aws_security_group"
```

## Commands Executed

```bash
cd /home/bob/terraform/t1q2

terraform init

terraform validate

terraform plan

terraform apply -auto-approve
```

## Concepts Learned

* Provider configuration
* Data Sources
* Resource references
* Key pair generation
* Local file generation
* Using `path.module`
* Implicit dependencies

---

# Task 2 — Attach Elastic IP

## Objective

Associate an Elastic IP with an EC2 instance.

## Terraform Resource

```hcl
aws_eip_association
```

## Commands

```bash
terraform plan

terraform apply
```

## Concepts Learned

* Resource association
* Dependency graph
* Referencing resource IDs
* Difference between creating resources and associating resources

---

# Task 3 — Attach IAM Policy

## Objective

Attach an IAM policy to an IAM user.

## Terraform Resource

```hcl
aws_iam_user_policy_attachment
```

## Commands

```bash
terraform plan

terraform apply
```

## Concepts Learned

* Managed policies
* IAM Policy ARN
* Resource referencing
* Policy attachment resource

---

# Task 4 — Create IAM User

## Objective

Provision an IAM user.

## Terraform Resource

```hcl
aws_iam_user
```

## Commands

```bash
terraform init

terraform apply
```

## Concepts Learned

* Basic resource creation
* Resource arguments
* Tags
* Global AWS resources

---

# Task 5 — Create DynamoDB Table

## Objective

Create a DynamoDB table using PAY_PER_REQUEST billing.

## Terraform Resource

```hcl
aws_dynamodb_table
```

## Concepts Learned

* Hash Key
* Attribute block
* Billing Mode
* String attributes

## Commands

```bash
terraform init

terraform apply
```

---

# Task 6 — Create Secrets Manager Secret

## Objective

Store application credentials securely.

## Terraform Resources

```hcl
aws_secretsmanager_secret

aws_secretsmanager_secret_version
```

## Concepts Learned

* Secret metadata
* Secret version
* JSON encoding
* Sensitive values

## Commands

```bash
terraform init

terraform apply
```

---

# Task 7 — Create Private S3 Bucket

## Objective

Provision a private S3 bucket.

## Terraform Resources

```hcl
aws_s3_bucket

aws_s3_bucket_public_access_block
```

## Concepts Learned

* Public Access Block
* Bucket configuration
* Multiple resources managing one service
* Resource references

## Commands

```bash
terraform init

terraform apply
```

---

# Task 8 — Create Public S3 Bucket

## Objective

Provision a publicly accessible S3 bucket.

## Terraform Resources

```hcl
aws_s3_bucket

aws_s3_bucket_acl

aws_s3_bucket_ownership_controls

aws_s3_bucket_public_access_block
```

## Concepts Learned

* ACL
* Ownership Controls
* Explicit dependencies using `depends_on`
* Provider v5 changes

## Commands

```bash
terraform init

terraform apply
```

---

# Task 9 — Create VPC

## Objective

Provision an IPv4 VPC.

## Terraform Resource

```hcl
aws_vpc
```

## Concepts Learned

* CIDR Block
* VPC resource
* Resource tagging

## Commands

```bash
terraform init

terraform apply
```

---

# Task 10 — Create IPv6-enabled VPC

## Objective

Provision a VPC with an Amazon-generated IPv6 CIDR block.

## Terraform Resource

```hcl
aws_vpc
```

## Important Argument

```hcl
assign_generated_ipv6_cidr_block = true
```

## Concepts Learned

* IPv6 support
* Dual-stack networking
* Optional resource arguments

## Commands

```bash
terraform init

terraform apply
```

---

# Terraform Commands Practiced

```bash
terraform init

terraform validate

terraform plan

terraform apply

terraform apply -auto-approve

terraform state list
```

---

# Terraform Concepts Revised

* Terraform workflow
* Provider block
* Resource block
* Data source
* Resource references
* Resource attributes
* Tags
* Variables vs hardcoded values
* JSON encoding
* Implicit dependency
* Explicit dependency (`depends_on`)
* Terraform state
* Resource association
* Sensitive resources
* Infrastructure as Code (IaC)

---

# Terraform Resources Practiced

```text
aws_instance

aws_key_pair

tls_private_key

local_file

aws_eip_association

aws_iam_user

aws_iam_policy

aws_iam_user_policy_attachment

aws_dynamodb_table

aws_secretsmanager_secret

aws_secretsmanager_secret_version

aws_s3_bucket

aws_s3_bucket_public_access_block

aws_s3_bucket_acl

aws_s3_bucket_ownership_controls

aws_vpc
```

---

# Issues Encountered & Resolution

### 1. Existing Resource Association

Some tasks required attaching Terraform-managed resources (such as an Elastic IP or IAM Policy) to existing resources. This reinforced how Terraform manages relationships using resource references and highlighted that, in production environments, existing resources typically need to be imported into the Terraform state before management.

### 2. Public S3 Bucket with AWS Provider v5

Using only:

```hcl
acl = "public-read"
```

was insufficient with AWS Provider v5. The solution required configuring ownership controls, adjusting the public access block settings, and then applying the ACL. This clarified how provider version changes can affect Terraform resource behavior.

### 3. Understanding `path.module`

Learned that `path.module` points to the directory containing the current Terraform module, making file paths portable instead of relying on hardcoded absolute paths.