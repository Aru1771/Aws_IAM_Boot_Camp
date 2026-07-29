Permissions Boundaries
=====================

A Permissions Boundary defines the maximum permissions an IAM user or role can ever receive.

Think of it as a ceiling.

Even if an attached IAM policy grants more permissions, the boundary prevents exceeding that limit.

Example
-------
Attached IAM Policy:

{
  "Effect": "Allow",
  "Action": "*",
  "Resource": "*"
}

Permissions Boundary:
----------------------
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Effect":"Allow",
      "Action":[
        "ec2:*",
        "s3:*"
      ],
      "Resource":"*"
    }
  ]
}

Effective permissions:
------------------------
EC2 ✅

S3 ✅

IAM ❌

Lambda ❌

CloudFormation ❌

Production Use Case
----------------------
A platform team allows developers to create IAM roles.

A Permissions Boundary ensures every new role is limited to:

EC2
S3
CloudWatch

Developers cannot accidentally or intentionally create administrator roles.
