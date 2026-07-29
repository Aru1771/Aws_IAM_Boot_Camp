Hands-on Lab
=============

Lab 1 – Permissions Boundary
------------------------------

Create a customer-managed policy named DeveloperBoundary:
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
Create a new IAM role.
Attach AdministratorAccess.
Set DeveloperBoundary as the Permissions Boundary.
Test:
Launch an EC2 instance ✅
Create an S3 bucket ✅
Create a new IAM user ❌ (AccessDenied)

Observe how the boundary limits the role despite the broad attached policy.

Lab 2 – SCP (requires AWS Organizations)
--------------------------------------------
If you have access to an AWS Organization:

Create a test OU.
Move a sandbox account into it.
Attach an SCP denying iam:*.
Log into the sandbox account with an administrator.
Attempt to create an IAM user and observe the AccessDenied error caused by the SCP.
