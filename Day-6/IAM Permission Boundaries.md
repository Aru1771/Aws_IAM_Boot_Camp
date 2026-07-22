IAM Permission Boundaries
=========================

Many engineers confuse this topic.

A Permission Boundary defines the maximum permissions an IAM User or Role can ever receive.

Think of it as a ceiling.

Example:

Permission Boundary:

Only EC2
Only S3

Administrator accidentally attaches:

AdministratorAccess

Effective permissions are still limited by the boundary.

Real-World Example

Company policy:

Developers must never create IAM Users.

Permission Boundary:

Deny IAM
Allow EC2
Allow S3

Even if someone attaches AdministratorAccess later, IAM operations remain blocked.
