1. Imagine this situation:

EC2 has an IAM Role attached with AmazonS3ReadOnlyAccess.
You SSH into the EC2 instance and run:
aws configure

Then you enter another IAM user's Access Key and Secret Key, and that IAM user has AdministratorAccess.

Question:

When you run:

aws s3 ls

or

aws ec2 describe-instances

which credentials will the AWS CLI use?

The IAM Role attached to the EC2 instance
The credentials configured with aws configure

Explain why. This is another common DevOps interview question.

Answer:
---------
"The AWS CLI will use the credentials configured by aws configure because the AWS credential provider chain checks local configured credentials
before checking the EC2 Instance Metadata Service (IMDS). It is not based on which identity has more permissions."

=====================================================================================================================================================================

2. If an EC2 instance has an IAM Role attached, where exactly does the AWS CLI get the temporary credentials from?

A. "When an EC2 instance has an IAM Role attached, the AWS CLI automatically retrieves temporary credentials from the EC2 Instance Metadata Service (IMDS).
Those credentials are issued by AWS STS for the attached IAM Role and are automatically refreshed before they expire."

=====================================================================================================================================================================
