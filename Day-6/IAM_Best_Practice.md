Enterprise IAM Best Practices
===============================
Root User
Enable MFA
Never use daily
No Access Keys
IAM Users
Individual user accounts
No shared credentials
IAM Groups

Create groups like:

Developers

DevOps

Security

QA

Assign permissions to groups, not individual users.

IAM Roles

Use roles for:

EC2
Lambda
Jenkins
Terraform
GitHub Actions
EKS Pods (IRSA)

Avoid long-term access keys wherever possible.

MFA

Enable MFA for:

Root User
Administrators
Privileged IAM Users
Password Policy
Minimum 12–14 characters
Complexity requirements
Prevent password reuse
Rotate according to organizational policy
Monitoring

Enable:

AWS CloudTrail
AWS Config
IAM Access Analyzer
CloudWatch Alarms
