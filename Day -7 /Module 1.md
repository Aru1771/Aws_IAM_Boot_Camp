Why Do We Need Cross-Account Access?
=====================================

Imagine your company has one AWS account.

AWS Account

EC2

S3

IAM

VPC

Everything is simple.

Now imagine the company grows.

Instead of one account, they create:
--------------------------------------
Development Account

Testing Account

Staging Account

Production Account

Security Account

Question:
---------
Can a DevOps engineer in the Development account directly manage resources in the Production account?

No.

AWS accounts are isolated by design.

Logging Account

Shared Services Account
