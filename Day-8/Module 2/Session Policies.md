Session Policies
=================

Session Policies apply only to temporary credentials.

They further restrict permissions during a specific session.

Common with:
------------
AssumeRole
AWS STS
AWS IAM Identity Center (SSO)

Example
--------
Role policy:
---------------
Allow S3
Allow EC2
Allow Lambda

Session Policy:
---------------
Allow only S3

Effective permissions:
-----------------------
S3 ✅

EC2 ❌

Lambda ❌

Session policies cannot



Production Example
--------------------
A CI/CD pipeline assumes a deployment role.

Normally the role can deploy to all environments.

For a production deployment, the pipeline uses a session policy allowing access only to:

Production S3 Bucket

Production ECR Repository

Even though the role has broader permissions, the temporary session is restricted.grant new permissions. They only reduce the permissions already available.
