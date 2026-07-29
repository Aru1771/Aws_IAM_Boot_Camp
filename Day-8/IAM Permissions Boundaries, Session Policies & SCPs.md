IAM Permissions Boundaries, Session Policies & Organizations SCPs
==================================================================

This is one of the most important topics for 4–8 years DevOps Engineer interviews because it explains how AWS limits permissions at multiple levels.

Today's Objectives
---------------------
By the end of today, you'll understand:

1. Permissions Boundaries
2. Session Policies
3. Service Control Policies (SCPs)
4. How all IAM policy types work together
5. Production use cases
6. Troubleshooting AccessDenied errors
7. Interview questions
   
Why Do We Need More Than IAM Policies?
----------------------------------------
Suppose you allow developers to create IAM roles.

Without restrictions, a developer could create this role:

{
  "Effect": "Allow",
  "Action": "*",
  "Resource": "*"
}

That effectively gives them Administrator access.

AWS solves this with Permissions Boundaries.


Permission Evaluation Layers
--------------------------------

               SCP
                │
                ▼
      Permission Boundary
                │
                ▼
          IAM Policies
                │
                ▼
        Session Policies
                │
                ▼
     Resource-based Policies
                │
                ▼
            Final Decision


Every layer can restrict access.
