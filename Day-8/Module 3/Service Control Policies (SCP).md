Service Control Policies (SCP)
=============================

SCPs are part of AWS Organizations.
----------------------------------
They apply to:

Organizational Units (OUs)
AWS Accounts

They define the maximum permissions available within those accounts.

Example Organization
Organization

├── Production OU
│      ├── Account A
│      └── Account B
│
└── Development OU
       ├── Account C
       └── Account D

Attach this SCP to the Development OU:

{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Effect":"Deny",
      "Action":"iam:*",
      "Resource":"*"
    }
  ]
}

Result:
--------
Every account in the Development OU is prevented from performing IAM actions, regardless of attached IAM policies.

Policy Evaluation Flow

When a request is made:

User

↓

Authentication

↓

SCP Check

↓

Permission Boundary

↓

IAM Policies

↓

Session Policy

↓

Resource Policy

↓

Explicit Deny?

↓

Allow or Deny

An explicit Deny at any applicable layer overrides all Allows.

Real Production Scenario
-------------------------
A developer reports:

AccessDenied

ec2:RunInstances

You check:

IAM Policy → Allow ✅
EC2 Resource Policy → Not applicable ✅
Permissions Boundary → Doesn't allow ec2:RunInstances ❌

Root cause:
------------
The Permissions Boundary blocks the action.

Another Scenario
-------------------
Administrator cannot create IAM users in a member account.

Checks:

IAM Policy → Allow ✅
SCP → Deny iam:* ❌

Root cause:
-----------------
The SCP overrides the IAM policy.
