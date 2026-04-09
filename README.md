# IAM Automation Lab (AWS CloudFormation + GitSync)

## Overview
This project demonstrates automated creation and management of AWS IAM resources using CloudFormation integrated with GitSync. The stack provisions IAM users, groups, and policies with secure credential handling via AWS Secrets Manager.

## Architecture
- CloudFormation template defines all IAM resources
- GitSync links the GitHub repository to the CloudFormation stack
- Secrets Manager generates and stores a temporary shared password

## Resources Created
### IAM Users
- ec2-user1 (EC2 access)
- ec2-user2 (EC2 access)
- s3-user1 (S3 access)

### IAM Groups
- EC2UserGroup (EC2 permissions)
- S3UserGroup (S3 permissions)

### Policies
- EC2GroupAccess: Allows describing and launching EC2 instances
- S3ReadOnlyAccess: Allows listing S3 buckets
- IAMUserChangePassword: Allows users to change their own passwords

### Secrets Manager
- TemporaryCredentials: Stores auto-generated password used for all users

## Permissions Model
- EC2 users can:
  - View EC2 instances
  - Launch EC2 instances
  - Cannot access S3

- S3 user can:
  - List S3 buckets
  - Cannot access EC2

## Deployment
The stack is deployed using GitSync.

### Deployment File
```
template-file-path: cloudformation/policy.yaml
stack-name: IAM-automation-lab
```

### Steps
1. Push changes to the repository
2. GitSync automatically updates the CloudFormation stack

## Testing
Each user is tested via AWS Console login.

Expected results:

| User | EC2 Access | S3 Access |
|------|-----------|----------|
| ec2-user1 | Success | Access Denied |
| ec2-user2 | Success | Access Denied |
| s3-user1 | Access Denied | Success |

Screenshots should be captured for each access attempt.

## Security Considerations
- Passwords are generated securely using AWS Secrets Manager
- Users are required to change password at first login
- Least privilege access is enforced via IAM groups

## Repository Structure
```
iam-automation/
├── deployment-file.yaml
└── cloudformation/
    └── policy.yaml
```

## Conclusion
This project demonstrates infrastructure as code principles applied to IAM resource management, along with automated deployment using GitSync and secure credential handling.