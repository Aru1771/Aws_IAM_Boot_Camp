Goal: By the end of today, you'll be able to write your own IAM policies and understand how AWS decides whether to allow or deny a request.
===================================================================

What is an IAM Policy?

An IAM Policy is a JSON document that defines permissions.

Without a policy:

IAM User
      │
      ▼
No Permissions

With a policy:

IAM User
      │
      ▼
IAM Policy
      │
      ▼
Can access AWS Services

Remember:
Policies answer one question: What actions is this identity allowed to perform on which resources?

Anatomy of a Policy
====================

Every policy has these main elements:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "...",
      "Resource": "..."
    }
  ]
}


Let's understand each field.

1. Version
===========
"Version": "2012-10-17"

This is the policy language version.

Use:

2012-10-17

for all new policies.

2. Statement
=============
A policy can have one or more statements.

Example:

Policy
   │
   ├── Statement 1
   ├── Statement 2
   └── Statement 3

Each statement defines one permission rule.

3. Effect
==========
There are only two values:

Allow

Deny

Example:

"Effect": "Allow"

or

"Effect": "Deny"

4. Action
==========
This defines what operations are allowed.

Examples:

ec2:StartInstances

ec2:StopInstances

ec2:DescribeInstances

s3:GetObject

s3:PutObject

Multiple actions:

"Action": [
    "ec2:StartInstances",
    "ec2:StopInstances"
]



5. Resource
=============
This defines which AWS resource the policy applies to.



This defines which AWS resource the policy applies to.

Example:
---------
"Resource": "*"

Means:

All resources.

More secure:
------------
Only one S3 bucket

Only one EC2 instance

Only one DynamoDB table



Example 1 - Read Only EC2
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Effect":"Allow",
      "Action":[
        "ec2:DescribeInstances"
      ],
      "Resource":"*"
    }
  ]
}


Example 1 - Read Only EC2
==========================

{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Effect":"Allow",
      "Action":[
        "ec2:DescribeInstances"
      ],
      "Resource":"*"
    }
  ]
}

Meaning:

✔ Can view EC2 instances

❌ Cannot start them

❌ Cannot stop them

❌ Cannot terminate them

Example 2 - Start and Stop EC2
==============================
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Effect":"Allow",
      "Action":[
        "ec2:StartInstances",
        "ec2:StopInstances"
      ],
      "Resource":"*"
    }
  ]
}

Wildcards
=========
Instead of listing every action:

ec2:StartInstances

ec2:StopInstances

ec2:RebootInstances

You can use:
-----------
"Action":"ec2:*"

Be careful—this grants all EC2 actions, so use it only when appropriate.



Explicit Deny
---------------
One of the most important IAM interview topics.

Suppose a user has:

Policy A

Allow S3 Full Access

Policy B

Deny DeleteObject

Result:

DeleteObject

↓

DENIED

Rule to remember:

Explicit Deny always overrides Allow.


IAM Policy Evaluation

When AWS receives a request, it evaluates permissions in this order:

User makes request
        │
        ▼
Is there an Explicit Deny?
        │
   Yes ─────► DENY
        │
       No
        ▼
Is there an Allow?
        │
   Yes ─────► ALLOW
        │
       No
        ▼
Implicit Deny

Three rules to memorize:

Explicit Deny → Wins
Explicit Allow → Grants access
No Allow → Implicit Deny

Managed vs Inline Policies
============================
1. AWS Managed Policy
------------------
Created and maintained by AWS.

Example:
---------
AmazonEC2ReadOnlyAccess
AmazonS3ReadOnlyAccess

2. Customer Managed Policy
   -----------------------

Created and managed by your organization.

Example:

MyCompany-DeveloperPolicy

Reusable across multiple users, groups, and roles.

3. Inline Policy
   -------------

Attached directly to a single user, group, or role.

Not reusable.

In production, customer-managed policies are generally preferred over inline policies because they're easier to reuse, audit, and version.


Hands-on Lab
=============
Task 1
---------
Create a customer-managed policy named:

DeveloperEC2ReadOnly

Permissions:

ec2:DescribeInstances

Attach it to:

Developers Group

Verify that users can view EC2 instances but cannot start or stop them.

Task 2
---------
Create another customer-managed policy that allows:

s3:ListBucket
s3:GetObject

Only for one bucket (replace with your bucket ARN).
