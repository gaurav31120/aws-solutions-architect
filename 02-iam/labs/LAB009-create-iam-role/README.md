# Lab: Create IAM Role

## Lab Number

**L009**

## Topic

**IAM — Identity and Access Management**

## Objective

The objective of this lab is to:

- Understand what an IAM role is.
- Understand how an IAM role differs from an IAM user.
- Create an IAM role trusted by Amazon EC2.
- Attach an AWS managed policy to the role.
- Understand the difference between a role's trust policy and permission policy.
- Prepare the role for the later EC2 → S3 access lab.

---

# 1. Real-World Story — GauravMart

Imagine GauravMart has an EC2 server.

The EC2 server needs to read files from Amazon S3.

One approach would be to put AWS credentials directly inside the server.

We do not want to do that.

Instead, AWS provides IAM Roles.

We can create a role and allow EC2 to use it:

    EC2
     |
     | assumes
     ↓
    IAM Role
     |
     | has permissions
     ↓
    Amazon S3

This allows an AWS service such as EC2 to obtain the permissions it needs without putting permanent AWS credentials inside the server.

---

# 2. What Is an IAM Role?

An IAM Role is an identity with permissions that can be assumed by a trusted entity.

For this lab:

    EC2
      ↓
    assumes
      ↓
    EC2-S3-ReadOnly-Role

The role then provides the permissions attached to it.

---

# 3. IAM User vs IAM Role

We have now learned both concepts.

## IAM User

An IAM user represents an identity such as a person or application.

Example:

    aws-learning-user

Think:

    Gaurav
       ↓
    IAM User

---

## IAM Role

A role is an identity that can be assumed by a trusted entity.

Example:

    EC2
      ↓
    IAM Role

In this lab:

    EC2-S3-ReadOnly-Role

---

# 4. Two Important Parts of an IAM Role

An IAM role has two important concepts.

## 4.1 Trust Policy

The trust policy answers:

> Who is allowed to assume this role?

For our role:

    EC2
      ↓
    Can assume the role

The trust relationship contains:

    "Action": [
        "sts:AssumeRole"
    ]

and identifies EC2 as the trusted AWS service.

Therefore:

    EC2
      ↓
    Trusted to assume
      ↓
    EC2-S3-ReadOnly-Role

---

## 4.2 Permission Policy

The permission policy answers:

> What can the role do after it is assumed?

For our role we attached:

    AmazonS3ReadOnlyAccess

Therefore:

    EC2
      ↓
    assumes role
      ↓
    EC2-S3-ReadOnly-Role
      ↓
    AmazonS3ReadOnlyAccess
      ↓
    S3 Read/List permissions

---

# 5. Trust vs Permission

This is the most important concept from this lab.

    TRUST POLICY
        ↓
    Who can assume the role?

    PERMISSION POLICY
        ↓
    What can the role do?

For our example:

    Trust:
    EC2 can assume the role

    Permission:
    Role can read S3

A role needs both concepts to work as intended.

---

# 6. Lab Architecture

The architecture created in this lab is:

    EC2
     |
     | Trusted to assume
     ↓
    EC2-S3-ReadOnly-Role
     |
     | Permission
     ↓
    AmazonS3ReadOnlyAccess
     |
     ↓
    Amazon S3

The role therefore connects:

    EC2
      ↓
    IAM Role
      ↓
    S3 permissions

---

# 7. Practical Lab

## Step 1 — Open IAM Roles

Navigated to:

    AWS Console
        ↓
    IAM
        ↓
    Roles

Selected:

    Create role

---

## Step 2 — Select Trusted Entity

Selected:

    Trusted entity type
        ↓
    AWS service

Selected the use case:

    EC2

This means EC2 is trusted to assume the role.

---

## Step 3 — Add Permissions

Selected the AWS managed policy:

    AmazonS3ReadOnlyAccess

This provides the role with S3 read/list permissions.

---

## Step 4 — Name the Role

Created the role with the name:

    EC2-S3-ReadOnly-Role

---

## Step 5 — Review and Create

Reviewed the role configuration and created the role.

AWS successfully displayed:

    Role EC2-S3-ReadOnly-Role created.

---

# 8. Important Configuration

## Role Name

    EC2-S3-ReadOnly-Role

## Trusted Entity

    EC2

## Permission Policy

    AmazonS3ReadOnlyAccess

## Trust Action

    sts:AssumeRole

---

# 9. Trust Policy Observed

The trust policy contained the EC2 service as the principal:

    "Principal": {
        "Service": [
            "ec2.amazonaws.com"
        ]
    }

It also contained:

    "Action": [
        "sts:AssumeRole"
    ]

This means EC2 is trusted to assume the role.

---

# 10. Permission Flow

The complete permission flow is:

    EC2
      |
      | assumes
      ↓
    EC2-S3-ReadOnly-Role
      |
      | has
      ↓
    AmazonS3ReadOnlyAccess
      |
      ↓
    S3 List / Read permissions

---

# 11. What I Tested

I verified that:

- The IAM role was successfully created.
- The role name is `EC2-S3-ReadOnly-Role`.
- EC2 is shown as the trusted entity.
- The role uses an EC2 trust relationship.
- `AmazonS3ReadOnlyAccess` was selected as the permission policy.
- The role appears in the IAM Roles list.

---

# 12. What Failed

No configuration failure occurred during the lab.

The role was successfully created.

---

# 13. How I Fixed It

No fix was required.

The role was created successfully with:

    Trusted entity:
    EC2

    Permission:
    AmazonS3ReadOnlyAccess

---

# 14. What I Learned

### 1. IAM Role

An IAM role is an identity that can be assumed by a trusted entity.

### 2. Trust Policy

The trust policy determines who can assume the role.

For this lab:

    EC2 → trusted

### 3. Permission Policy

The permission policy determines what the role can do.

For this lab:

    AmazonS3ReadOnlyAccess

### 4. Roles avoid embedding permanent credentials

Instead of putting AWS credentials directly into an EC2 server, the server can use an IAM role.

### 5. Trust and permissions are different

Remember:

    Trust Policy
        ↓
    Who can assume me?

    Permission Policy
        ↓
    What can I do?

---

# 15. Security Principle

## Avoid Hard-Coding AWS Credentials

For AWS services such as EC2, prefer IAM roles instead of storing long-term AWS credentials inside the application or server.

The desired architecture is:

    EC2
      ↓
    IAM Role
      ↓
    AWS Service Permissions

instead of:

    EC2
      ↓
    Hard-coded AWS credentials

---

# 16. SAA Takeaway

For the AWS Solutions Architect Associate exam, remember:

    IAM User
        ↓
    Represents an identity

    IAM Role
        ↓
    Can be assumed by a trusted entity

The two most important role concepts are:

    Trust Policy
        ↓
    Who can assume the role?

    Permission Policy
        ↓
    What can the role access?

For this lab:

    EC2
      ↓
    assumes
      ↓
    EC2-S3-ReadOnly-Role
      ↓
    AmazonS3ReadOnlyAccess
      ↓
    S3 Read/List

This pattern will be used later in:

    EC2 → S3 Access

---

# 17. Cleanup

No EC2 instance or S3 bucket was created during this lab.

The IAM role is intentionally retained because it will be used in the upcoming IAM/EC2 exercises.

Role created:

    EC2-S3-ReadOnly-Role