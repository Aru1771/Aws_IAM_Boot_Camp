🎯 Topic: IAM Roles
===================== 
Goal

By the end of today, you should understand:

What an IAM Role is
IAM User vs IAM Role
Why IAM Roles are preferred over Access Keys
Temporary Credentials
STS (Security Token Service)
Instance Profile
EC2 → S3 access using IAM Role
Hands-on Lab

1. What is an IAM Role?
------------------------
An IAM Role is an AWS identity that does not have permanent credentials.

Instead of creating Access Keys, AWS provides temporary credentials whenever someone or something assumes the role.

Think of it like this:

IAM User
    ↓
Permanent Identity
Username + Password
Access Key
Secret Key


Whereas:

IAM Role
    ↓
No Password
No Permanent Access Keys
Temporary Credentials

2. Why Do We Need IAM Roles?
-------------------------------
Suppose you launch an EC2 instance.

Your application needs to read files from an S3 bucket.

Wrong Approach ❌

Store Access Keys inside the EC2 instance.

EC2
 ├── Access Key
 ├── Secret Key
 └── Application

 Problems:
-----------
Access keys can be leaked.
Keys need manual rotation.
If the instance is compromised, the attacker gets long-lived credentials.

Correct Approach ✅
-------------------
Attach an IAM Role to the EC2 instance.

EC2
   │
IAM Role
   │
Temporary Credentials
   │
S3

No Access Keys are stored on the server.


3. IAM User vs IAM Role
------------------------

| IAM User                    | IAM Role                                               |
| --------------------------- | ------------------------------------------------------ |
| Permanent identity          | Temporary identity                                     |
| Password/Login              | No login                                               |
| Access Keys                 | Temporary credentials                                  |
| Used by humans              | Used by AWS services, applications, or federated users |
| Credentials must be rotated | Credentials are automatically rotated by AWS           |

4. What is STS?
---------------
STS stands for:

Security Token Service

Its job is to issue:

Temporary AWS Credentials

Whenever a role is assumed.


5. Real Flow
------------
Suppose:

EC2
 ↓
Needs S3 Access

Flow:

EC2
 ↓
IAM Role
 ↓
STS
 ↓
Temporary Credentials
 ↓
S3

The application never stores Access Keys.

6. What Are Temporary Credentials?
------------------------------------
AWS automatically generates:

Access Key ID

Secret Access Key

Session Token

These expire automatically after a limited time.

Example:

Valid for 1 hour

After expiration:

AWS automatically refreshes them.

7. What is an Instance Profile?
-------------------------------

This is one of the most commonly asked interview questions.

Definition

An Instance Profile is a container for an IAM Role that allows an EC2 instance to use that role.

Think of it as:

IAM Role
      │
Instance Profile
      │
EC2 Instance

You don't attach the role directly to EC2. AWS attaches an Instance Profile, which contains the role.



8. Production Example
---------------------
Suppose you have:
Spring Boot Application
Running on EC2

It needs to:
Read Files
Write Logs
Upload Reports

Instead of configuring:
Access Key
Secret Key

You create:
IAM Role
↓
AmazonS3ReadOnlyAccess
Attach it to EC2.

Application simply calls:
AmazonS3ClientBuilder.defaultClient()
The AWS SDK automatically retrieves temporary credentials from the EC2 Instance Metadata Service (IMDS).


9. How Does EC2 Get Credentials?
   -----------------------------

   Application
      │
AWS SDK
      │
EC2 Metadata Service (IMDS)
      │
Instance Profile
      │
IAM Role
      │
STS
      │
Temporary Credentials
      │
   S3

This is completely automatic.



10. Why Roles Are More Secure
    --------------------------
Access Keys
Long-lived

Stored in files

Can leak

Manual rotation
IAM Roles
Temporary

Automatically rotated

No secrets stored


11. Real DevOps Examples
 -----------------------
Use IAM Roles for:

EC2 → S3
EC2 → DynamoDB
Lambda → S3
Lambda → SNS
ECS Task → S3
EKS Pods → AWS Services (using IRSA)
AWS Load Balancer Controller
cert-manager (Route53 DNS updates)
ExternalDNS
EBS CSI Driver
Recommended by AWS


🧪 Hands-on Lab 1
--------------------
Goal

Allow an EC2 instance to read from an S3 bucket without Access Keys.

Steps
Create an S3 bucket.
Create an IAM Role for EC2.
Attach the policy:
AmazonS3ReadOnlyAccess
Launch an EC2 instance.
Attach the IAM Role to the EC2 instance.
SSH into the EC2 instance.
Install AWS CLI (if not already installed).
Verify the identity:
aws sts get-caller-identity
List S3 buckets:
aws s3 ls

You should be able to access S3 without configuring:

aws configure

or creating Access Keys.

🧪 Hands-on Lab 2
-------------------------
Create a custom IAM policy that allows:

s3:GetObject
s3:ListBucket

Attach it to an IAM Role and verify that the EC2 instance can:

List the bucket.
Download objects.
Cannot upload or delete objects.

🎤 Interview Questions
--------------------------
Q1. Why do we use IAM Roles instead of Access Keys?

Answer:

IAM Roles provide temporary credentials through AWS STS, eliminating the need to store long-lived access keys. This improves security by enabling automatic credential rotation and reducing the risk of credential leakage.

Q2. What is an Instance Profile?

Answer:

An Instance Profile is a container for an IAM Role that allows an EC2 instance to obtain temporary AWS credentials from the Instance Metadata Service. Applications on the instance use these credentials automatically through the AWS SDK.

Q3. What service issues temporary credentials?

Answer:

AWS Security Token Service (STS).

📚 Homework
------------
Create an IAM Role for EC2.
Attach AmazonS3ReadOnlyAccess.
Attach the role to an EC2 instance.
Verify the role using:
aws sts get-caller-identity
Test access with:
aws s3 ls
