Complete Cross-Account Flow
============================

Suppose:
--------

Development Account

Account ID:
111111111111

↓

Production Account

Account ID:
222222222222

The flow is:
-------------
Developer

↓

Development IAM User

↓

sts:AssumeRole

↓

Production IAM Role

↓

Temporary Credentials


Steps:
-------
Developer authenticates in the Development account.
Calls sts:AssumeRole.
Production role checks its trust policy.
STS issues temporary credentials.
The developer now works in the Production account using those temporary credentials.
↓

Deploy Application
