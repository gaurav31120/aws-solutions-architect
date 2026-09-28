# Lab: Attach Managed Policy

## Lab Number

**L007**

## Topic

**IAM — Identity and Access Management**

## Objective

The objective of this lab is to:

- Understand what an IAM policy is.
- Understand what an AWS managed policy is.
- Attach a managed policy to an IAM group.
- Understand how permissions assigned to a group are inherited by its users.
- Practice permission management using a safe read-only policy.

---

# 1. Real-World Story — GauravMart

Imagine we have a company called **GauravMart**.

The company has several developers.

Instead of giving permissions individually to every developer, we create a group:

    GauravMart
       |
       +-- Developers Group
              |
              +-- Developer 1
              +-- Developer 2
              +-- Developer 3

Now suppose developers need to view files stored in Amazon S3.

Instead of giving the permission separately to every user:

    User 1 → S3 permission
    User 2 → S3 permission
    User 3 → S3 permission

we attach the permission to the group:

    Developers Group
           |
           +-- AmazonS3ReadOnlyAccess

Users belonging to the group receive the permissions provided by that policy.

This makes permission management easier.

---

# 2. What Is an IAM Policy?

An IAM policy is a document that defines:

    What can be done
          +
    On which AWS resources
          +
    Under which conditions

For example:

    Allow
       |
       +-- Read S3 objects

A policy determines what actions an IAM identity is allowed or denied to perform.

---

# 3. What Is a Managed Policy?

A **managed policy** is an IAM policy that can be attached to IAM identities such as:

- Users
- Groups
- Roles

AWS provides many AWS managed policies.

These policies are maintained by AWS and can be reused.

For this lab, we used:

    AmazonS3ReadOnlyAccess

This provides read-only access to Amazon S3.

---

# 4. AWS Managed Policy vs Inline Policy

Basic distinction:

    Managed Policy
         |
         +-- Reusable
         +-- Can be attached to multiple identities

    Inline Policy
         |
         +-- Embedded directly into one identity
         +-- Closely tied to that identity

For this lab, we are using an **AWS managed policy**.

---

# 5. Lab Architecture

The final IAM structure is:

    IAM
     |
     v
    Developers Group
       /          \
      /            \
     v              v
    aws-learning-user    Other Users
           |
           |
           v
    AmazonS3ReadOnlyAccess
           |
           v
    S3 Read Permissions

The important relationship is:

    User
      ↓
    Group
      ↓
    Managed Policy
      ↓
    Permissions

---

# 6. Practical Lab

## Step 1 — Open IAM

Open the AWS Console.

Navigate to:

    IAM
       ↓
    User groups

Open the existing:

    Developers

---

## Step 2 — Open Permissions

Inside the `Developers` group:

    Developers
       ↓
    Permissions

Open the **Permissions** tab.

---

## Step 3 — Add Permissions

Choose:

    Add permissions

Then select the option to attach policies directly.

---

## Step 4 — Search for the Managed Policy

Search for:

    AmazonS3ReadOnlyAccess

Select:

    AmazonS3ReadOnlyAccess

---

## Step 5 — Attach the Policy

Complete the AWS console flow to attach the policy.

The final group should contain:

    Developers
        |
        +-- Permissions policies
                |
                +-- AmazonS3ReadOnlyAccess

---

# 7. Important Configuration

### IAM Group

    Developers

### IAM User

    aws-learning-user

### Managed Policy

    AmazonS3ReadOnlyAccess

### Policy Type

    AWS managed policy

### Permission Level

    Read-only access to S3

We intentionally did not attach:

    AdministratorAccess

because the test user does not need administrator-level permissions for this lab.

---

# 8. Permission Flow

The most important concept from this lab is:

    aws-learning-user
            |
            v
       Developers
            |
            v
    AmazonS3ReadOnlyAccess
            |
            v
     S3 Read Permissions

The user receives the permissions associated with the group.

This allows permissions to be managed centrally.

---

# 9. What I Tested

I verified that:

- The `Developers` IAM group exists.
- `aws-learning-user` is a member of the group.
- The group has a permissions policy attached.
- The attached policy is `AmazonS3ReadOnlyAccess`.
- The policy is an AWS managed policy.
- The policy provides read-only S3 permissions.

---

# 10. What Failed

No failure occurred during this lab.

The policy was successfully attached to the `Developers` group.

---

# 11. How I Fixed It

No fix was required.

The policy attachment completed successfully.

---

# 12. What I Learned

### 1. Policies define permissions

An IAM policy determines what an identity can do.

### 2. Managed policies can be reused

A managed policy can be attached to IAM users, groups, or roles.

### 3. Groups simplify permission management

Instead of attaching the same permission separately to many users, the permission can be attached to a group.

### 4. Users inherit group permissions

A user belonging to a group receives the permissions associated with that group.

### 5. Permissions should follow least privilege

Users should receive only the permissions required for their work.

---

# 13. Security Principle

## Least Privilege

Do not give every user administrator access.

Instead:

    Required permission
           ↓
    Grant only that permission

For this learning lab:

    Developers
        ↓
    AmazonS3ReadOnlyAccess

was used instead of broad administrator permissions.

---

# 14. SAA Takeaway

For the AWS Solutions Architect Associate exam, remember:

    IAM User
        ↓
    can belong to
        ↓
    IAM Group
        ↓
    which can have
        ↓
    IAM Policies
        ↓
    which define
        ↓
    Permissions

### Key Exam Concept

**IAM policies determine what actions an identity is allowed or denied to perform.**

### Remember

    User  = Identity

    Group = Collection of users

    Policy = Permission rules

    Role  = Identity that can be assumed

Roles will be covered in a later IAM lab.

---

# 15. Cleanup

For this lab, no additional AWS infrastructure was created.

The managed policy can remain attached because it is a read-only AWS managed policy and will be used in the upcoming IAM permission-testing exercises.

If the lab environment is no longer needed, the policy can be detached from the `Developers` group.

---

# 16. Lab Status

**L007 — Attach Managed Policy**

    Status: ✅ COMPLETED

---

# 17. GitHub Location

    02-iam/
    └── labs/
        ├── LAB005-create-iam-test-user/
        │   └── README.md
        ├── LAB006-create-iam-group/
        │   └── README.md
        └── LAB007-attach-managed-policy/
            └── README.md

---

# 18. Git Commit

### Lab README

    feat(aws-iam-l007): attach managed policy

### PROBLEMS.md

    docs(aws-iam-l007): mark L007 completed

---

# 19. Next Lab

**L008 — Test IAM Permissions**

The next lab will test whether the permissions attached through the IAM group produce the expected access behavior.