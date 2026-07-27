Trust Policy vs Permission Policy
====================================

This is one of the most frequently asked interview topics.

Trust Policy
-------------
Answers:

Who can assume this role?

Example:
----------

{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Effect":"Allow",
      "Principal":{
        "AWS":"arn:aws:iam::111111111111:root"
      },
      "Action":"sts:AssumeRole"
    }
  ]
}

This means:
------------
Account 111111111111 is allowed to assume the role.


Permission Policy
-----------------
Answers:

What can the role do after it is assumed?

Example:

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

Think of it like this:
| Policy            | Purpose                               |
| ----------------- | ------------------------------------- |
| Trust Policy      | Who can enter the building?           |
| Permission Policy | What can they do inside the building? |
