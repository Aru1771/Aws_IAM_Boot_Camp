IAM Identity Center (AWS SSO)
================================
This is one of the most important IAM topics for modern AWS environments because most companies no longer create long-term IAM users for employees. 
Instead, they use IAM Identity Center.

Learning Objectives
--------------------
By the end of Day 5, you'll understand:

What is IAM Identity Center (formerly AWS SSO)?
Why companies prefer it over IAM Users
Identity Source (Internal Directory vs External IdP)
Permission Sets
Groups and Assignments
MFA Integration
Temporary Credentials
Login Flow
CLI Access using AWS SSO
Real production architecture
Hands-on lab
Interview questions

Why was IAM Identity Center introduced?
----------------------------------------
Imagine a company with 500 employees.

Without IAM Identity Center:

500 IAM Users

↓

500 Passwords

↓

500 MFA Devices

↓

Manual Permission Management

Problems:

Difficult to manage
Password rotation

AWS Solution
--------------
Instead of creating IAM Users,

create users in

IAM Identity Center

Users log in once

↓

Receive temporary credentials

↓

Access one or more AWS accounts.

Traditional IAM User Login
---------------------------
Developer

↓

IAM User

↓

Username

↓

Password

↓

MFA

↓

AWS Console

IAM Identity Center Login
--------------------------
Developer

↓

IAM Identity Center

↓

Username

↓

Password

↓

MFA

↓

Permission Set

↓

AWS Account

↓

IAM Role

↓

AWS Console

Notice:

There is no permanent IAM User in each AWS account for employees.


Core Components
------------------
1. Identity Source
-------------------
This is where users are managed.

Examples:

Internal Identity Center directory
Microsoft Active Directory
Microsoft Entra ID (Azure AD)
Okta
Google Workspace (via supported federation)

2. Users
----------
Example:

Aravind
Rahul
John

3. Groups
----------
Example:

Developers

DevOps

Security

Admins

4. Permission Sets
--------------------
This is one of the most important concepts.

A Permission Set is a template of IAM permissions.
Example:

AdministratorAccess

or

ReadOnlyAccess

or a custom permission set.

When assigned to an AWS account, AWS creates the required IAM Role automatically behind the scenes.


Login Flow
-----------
Developer

↓

IAM Identity Center Login

↓

Authentication

↓

MFA

↓

Permission Set

↓

Temporary IAM Role

↓

AWS Console

Why Temporary Credentials?
--------------------------
Unlike IAM Users:

Access Key

↓

Permanent

Identity Center gives:
-----------------------
Temporary Credentials

↓

Expire Automatically

This follows AWS security best practices.

Production Example
--------------------
Suppose your organization has:

AWS Organization

├── Dev Account
├── Test Account
├── Prod Account

Developer logs in once.

Depending on the assigned Permission Set:

Dev Account

↓

PowerUser

Prod Account

↓

ReadOnly

The same user gets different permissions in different AWS accounts.

CLI Access with IAM Identity Center
------------------------------------

Configure:
----------
aws configure sso

You'll provide:

SSO Start URL
AWS Region
Account
Role (Permission Set)

Login:
------
aws sso login

Verify:

aws sts get-caller-identity

Instead of using:

aws configure

with long-term access keys.

Real Company Architecture
---------------------------
Developer

↓

Corporate Login

↓

IAM Identity Center

↓

MFA

↓

Permission Set

↓

Temporary IAM Role

↓

AWS Account

↓

AWS Services

No long-term credentials.
