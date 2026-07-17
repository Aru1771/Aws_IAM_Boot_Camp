Trust Policies & AssumeRole
===============================
This is one of the most important IAM topics for DevOps engineers. It is used in:

1.EKS IAM Roles for Service Accounts (IRSA)
2.Cross-account deployments
3.AWS Organizations
4.GitHub Actions → AWS authentication
5.Jenkins → AWS authentication
6.Terraform deployments
7.AWS Identity Center (SSO)


Today's Learning Objectives
----------------------------
By the end of Day 4, you will understand:

What is an AssumeRole?
What is a Trust Policy?
Identity Policy vs Trust Policy
How AssumeRole works
Cross-account role assumption
External ID (concept)
Hands-on Lab
Interview Questions

1. Why Do We Need AssumeRole?
  ---------------------------
Imagine you have two AWS accounts.

Production Account
└── S3 Bucket

Development Account
└── EC2 Instance

The EC2 instance in the Development account needs to read an S3 bucket in the Production account.

Creating IAM users and sharing access keys is a bad practice.

Instead:

EC2
   │
Assume Role
   │
Production Role
   │
STS
   │
Temporary Credentials
   │
Access S3

This is the recommended AWS approach.



2. What is AssumeRole?
----------------------
AssumeRole is the process of temporarily becoming another IAM Role.

When a user, application, or AWS service assumes a role:

AWS verifies that the role trusts the caller.
AWS STS issues temporary credentials.
The caller uses those temporary credentials until they expire.

The original identity doesn't permanently change.


3. What is a Trust Policy?
-------------------------------
This is one of the most common interview questions.

An Identity Policy answers:

What actions can I perform?

A Trust Policy answers:

Who is allowed to assume this role?

Every IAM Role has a Trust Policy.

Example:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}

Meaning:

"Only the EC2 service is allowed to assume this role."

4. Identity Policy vs Trust Policy
   ------------------------------------

   | Identity Policy                      | Trust Policy                        |
| ------------------------------------ | ----------------------------------- |
| Attached to users, groups, or roles  | Attached only to IAM Roles          |
| Defines **what** actions are allowed | Defines **who** can assume the role |
| Example: `s3:GetObject`              | Example: `sts:AssumeRole` for EC2   |


A role usually needs both:

A trust policy so the right principal can assume it.
Permission policies so the role can perform actions after being assumed.


5. How AssumeRole Works
   ---------------------

   IAM User / EC2 / Lambda
          │
Requests sts:AssumeRole
          │
AWS checks Trust Policy
          │
Allowed?
     │         │
    Yes       No
     │         │
STS returns   Access Denied
temporary credentials

6. Real DevOps Example
 --------------------
A Jenkins server in one AWS account deploys infrastructure into another account.

Flow:

Jenkins
    │
IAM User or Role
    │
sts:AssumeRole
    │
Deployment Role (Target Account)
    │
Terraform
    │
Creates EC2, VPC, IAM, S3

No permanent keys from the target account are stored in Jenkins.


Hands-on Lab
-------------
Step 1
------
Create an IAM Role.

Trusted entity:

AWS Account

Choose another AWS account ID (or use a second account if you have one).

Step 2
------
Attach a simple permission policy:

AmazonS3ReadOnlyAccess
Step 3
-------
Review the Trust Policy and identify:

Who is the Principal?
Which action allows the role to be assumed?
Step 4 (CLI)
-------------
Assume the role:

aws sts assume-role \
  --role-arn arn:aws:iam::<ACCOUNT_ID>:role/<ROLE_NAME> \
  --role-session-name demo-session

Observe that the response contains:

AccessKeyId
SecretAccessKey
SessionToken
Expiration

These are temporary credentials issued by STS.

Interview Questions

Answer these tomorrow in your own words:

What is sts:AssumeRole?
What is a Trust Policy?
What is the difference between an Identity Policy and a Trust Policy?
Can a role be assumed without a Trust Policy?
Why is AssumeRole preferred over sharing access keys?
Which AWS service issues the temporary credentials after a role is assumed?

This topic is fundamental for advanced AWS authentication patterns you'll use with EKS, GitHub Actions, Jenkins, and cross-account deployments. Good luck with Day 4! 🚀
