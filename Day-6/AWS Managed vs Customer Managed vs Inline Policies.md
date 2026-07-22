AWS Managed vs Customer Managed vs Inline Policies
===================================================

AWS Managed Policy

Created by AWS.

Examples:

AmazonS3ReadOnlyAccess
AmazonEC2FullAccess
AdministratorAccess
Advantages
Maintained by AWS
Easy to use
Disadvantages
May grant more permissions than required
Customer Managed Policy

Created by your organization.

Example:

Developer-EC2-ReadOnly

Advantages:

Reusable
Version controlled
Follows Least Privilege

This is the preferred approach in production.

Inline Policy

Attached to only one:

User
Group
Role

Cannot be reused.

Mostly used for unique one-off permission requirements.

Interview Question

Which policy type is recommended in production?

Answer:

Customer Managed Policies because they are reusable, version-controlled, and can be tailored to least-privilege requirements.
