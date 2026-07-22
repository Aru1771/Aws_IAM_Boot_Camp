Real Production IAM Architecture
=================================
AWS Account
      │
      ├── Root User (MFA Enabled)
      │
      ├── IAM Identity Center (SSO)
      │
      ├── Developers Group
      │
      ├── DevOps Group
      │
      ├── Security Group
      │
      ├── EC2 Roles
      │
      ├── Lambda Roles
      │
      ├── Jenkins Role
      │
      └── EKS IRSA Roles

This is close to what you'll find in enterprise AWS environments.
