What Happens During AssumeRole?
================================

Suppose you're logged into the Development account and need to deploy to Production.

Developer
     │
     ▼
Development Account
     │
sts:AssumeRole
     ▼
Production Role
     │
Temporary Credentials
     ▼
Production Resources

The role in the Production account decides whether to trust you.
