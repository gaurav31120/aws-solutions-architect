# LAB005 — Create IAM Test User

## 🎯 Objective

Create a basic IAM User in AWS and understand the difference between:

- IAM User
- Identity
- Permissions
- IAM Policies
- Root User
- IAM Groups
- Least Privilege

The main goal of this lab is to understand how AWS identifies a person or application.

---

## 🧠 Real-World Story

Imagine that we have a company called **GauravMart**.

GauravMart runs its application on AWS.

The company has an AWS account.

Inside the company, different employees need different levels of access.

For example:

    🏢 GAURAVMART
         │
         ▼
      AWS ACCOUNT
         │
         ▼
        🛡️ IAM
         │
    ┌────┼────┐
    │    │    │
    ▼    ▼    ▼
    👨   👩   👨
  Gaurav Priya Rahul
 Developer Tester Developer

We should not give every employee complete access to AWS.

IAM helps us control:

    WHO can access AWS?
             +
    WHAT can they do?

---

# 💡 What is IAM?

IAM stands for:

**Identity and Access Management**

In simple words:

> IAM controls who can access AWS and what they are allowed to do.

Think of IAM as the **security department of our AWS account**.

    AWS ACCOUNT
         │
         ▼
       🛡️ IAM
         │
         ├── 👤 Who are you?
         │
         └── 🔐 What can you do?

---

# 👤 What is an IAM User?

An IAM User represents an identity inside an AWS account.

For example:

    👤 IAM USER
         │
         ▼
    aws-learning-user

This tells AWS:

> "This is a particular identity."

However, creating a user does NOT automatically mean that the user can perform every AWS operation.

This is an important concept.

    👤 IAM USER
         │
         │ represents
         ▼
      IDENTITY

    📜 IAM POLICY
         │
         │ defines
         ▼
     PERMISSIONS

Therefore:

> **Identity and permission are different concepts.**

---

# 👑 Root User vs IAM User

When an AWS account is created, AWS provides a special identity called the **Root User**.

Think of the Root User as the owner of the entire AWS account.

    🏢 AWS ACCOUNT
         │
         ├── 👑 ROOT USER
         │
         └── 🛡️ IAM
               │
               ├── 👤 IAM USERS
               ├── 👥 IAM GROUPS
               └── 🎭 IAM ROLES

The Root User should not be used for normal everyday AWS administration.

Instead, appropriate IAM identities should be used for normal work.

---

# 📜 What is an IAM Policy?

An IAM Policy is a document that defines permissions.

Think about it like a rule book.

For example:

    📜 POLICY

    Allow:
        Read S3 files

    Do not allow:
        Delete S3 files

The policy determines what actions an identity is allowed or denied to perform.

The relationship is:

    👤 USER
       │
       ▼
    📜 POLICY
       │
       ▼
    🔐 PERMISSIONS
       │
       ▼
    AWS RESOURCES

We will study IAM Policies in later labs.

---

# 👥 What is an IAM Group?

Imagine our company has 100 developers.

Instead of managing permissions separately for every developer, we can create a group.

    👥 DEVELOPERS
          │
      ┌───┼───┐
      │   │   │
      ▼   ▼   ▼
      👤  👤  👤
    Gaurav Priya Rahul

The group can have permissions appropriate for developers.

Users can then be added to the group.

This makes permission management easier.

We will practice IAM Groups in:

**LAB006 — Create IAM Group**

---

# 🔐 Least Privilege

One of the important AWS security principles is:

> Give an identity only the permissions it actually needs.

For example, if an application only needs to read files from S3, we should not automatically give it permissions to:

- Delete S3 buckets
- Delete EC2 instances
- Modify IAM users
- Access unrelated AWS services

Instead:

    🔐 LEAST PRIVILEGE
           │
           ▼
    Give only the permissions
    that are actually required.

This principle is extremely important in AWS security.

---

# 🧪 Practical Lab

## Step 1 — Open AWS IAM

Open the AWS Management Console.

Search for:

**IAM**

Open:

**IAM — Identity and Access Management**

---

## Step 2 — Open IAM Users

From the IAM navigation menu:

**IAM → Users**

Before this lab, the account had:

**IAM Users = 0**

---

## Step 3 — Create the User

Click:

**Create user**

For the username, enter:

**aws-learning-user**

The identity we are creating is therefore:

    🛡️ IAM
       │
       ▼
    👤 IAM USER
       │
       ▼
    aws-learning-user

---

## Step 4 — Console Access

AWS provides an option:

**Provide user access to the AWS Management Console**

For this lab, console access was not enabled.

The purpose of this lab was to understand the creation of an IAM identity first.

---

## Step 5 — Set Permissions

AWS provides different ways to configure permissions.

The options shown were:

1. Add user to group
2. Copy permissions
3. Attach policies directly

For this lab, none of these permission options were selected.

We will learn IAM Groups and IAM Policies separately in later labs.

---

## Step 6 — Permissions Boundary

AWS also provides:

**Set permissions boundary — optional**

No permissions boundary was configured for this lab.

Permissions boundaries are an advanced IAM concept and will be studied later.

---

## Step 7 — Review and Create

The configuration was reviewed.

The username was:

**aws-learning-user**

The user was then created.

---

# 🏗️ Result

After completing the lab, our AWS account contains:

    🏢 AWS ACCOUNT
          │
          ▼
        🛡️ IAM
          │
          ▼
    👤 IAM USER
          │
          ▼
    aws-learning-user

The user now exists as an IAM identity.

No permissions were attached during this lab.

Permissions will be studied in subsequent IAM labs.

---

# ✅ Verification

After creating the user:

1. Opened **IAM**.
2. Opened **Users**.
3. Verified that the user appears in the list.
4. Confirmed the username:

**aws-learning-user**

The IAM user count changed from:

**0 → 1**

Therefore, the user was successfully created.

---

# 🔐 Security Notes

Never commit AWS credentials or secrets to GitHub.

Never commit:

- AWS passwords
- Access Key IDs
- Secret Access Keys
- Temporary credentials
- MFA recovery information
- Other AWS secrets

Only document the configuration and learning outcomes.

---

# 🧠 What I Learned

- IAM stands for Identity and Access Management.
- IAM controls access to AWS resources.
- An IAM User represents an identity.
- Creating an IAM User does not automatically give it permissions.
- IAM Policies define permissions.
- IAM Groups can be used to manage permissions for multiple users.
- The Root User is the owner-level identity of an AWS account.
- The Root User should not be used for normal everyday AWS operations.
- Least privilege means giving only the permissions that are actually required.
- Permission boundaries are an advanced IAM concept.

---

# 🎯 Key Takeaway

Remember this simple rule:

    👤 IAM USER
         =
    WHO ARE YOU?


    📜 IAM POLICY
         =
    WHAT ARE YOU ALLOWED TO DO?

The overall IAM picture:

    🛡️ IAM
       │
       ├── 👤 Users
       │
       ├── 👥 Groups
       │
       └── 🎭 Roles
              │
              ▼
         📜 Policies
              │
              ▼
         🔐 Permissions
              │
        ┌─────┼─────┐
        ▼     ▼     ▼
       EC2   S3    RDS

---

# 📌 Lab Status

**LAB005 — ✅ COMPLETED**

Created IAM User:

**aws-learning-user**

---

# ⏭️ Next Lab

**LAB006 — Create IAM Group**

---