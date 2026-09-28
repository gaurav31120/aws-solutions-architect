# Lab 011 — Use AWS CLI

## Objective

Use the AWS Command Line Interface (CLI) to interact with AWS services using the IAM user `aws-learning-user`.

The goals of this lab were to:

- Verify the configured AWS CLI profile.
- Use AWS CLI to interact with Amazon S3.
- Use AWS CLI to interact with IAM.
- Observe how IAM permissions control CLI operations.
- Understand the difference between successful authentication and authorization.
- Observe an `AccessDenied` error caused by missing IAM permissions.

---

## Real-World Story

Imagine GauravMart has an AWS account.

An employee has an AWS identity:

```text
aws-learning-user
```

The employee has a security badge that allows access to certain areas of the warehouse.

The employee can enter the S3 storage area, but cannot enter the IAM administration area.

The AWS CLI behaves in the same way.

```text
AWS CLI
   │
   ▼
Who are you?
   │
   ▼
aws-learning-user
   │
   ▼
What permissions do you have?
   │
   ├───────────────┐
   ▼               ▼
S3              IAM
Allowed         Restricted
```

Having valid credentials does not automatically give an identity permission to perform every AWS operation.

---

## Concept — Using AWS CLI

AWS CLI allows AWS services to be accessed from the command line.

For example:

```powershell
aws s3 ls --profile aws-learning-user
```

The command sends a request to Amazon S3 using the credentials configured for the specified AWS CLI profile.

AWS then evaluates the IAM permissions associated with that identity.

---

## Authentication vs Authorization

This lab demonstrated an important IAM distinction.

### Authentication

Authentication answers:

> Who are you?

This was verified using:

```powershell
aws sts get-caller-identity --profile aws-learning-user
```

AWS returned the identity associated with the configured credentials.

---

### Authorization

Authorization answers:

> What are you allowed to do?

The CLI commands in this lab demonstrated that the authenticated user could perform some operations but could not perform IAM operations that were not included in its permissions.

---

## Current IAM Permission Structure

The IAM configuration used for this lab was:

```text
aws-learning-user
       │
       ▼
   Developers
       │
       ▼
AmazonS3ReadOnlyAccess
```

The user therefore has permissions provided through the `Developers` group.

The attached managed policy is focused on Amazon S3 read/list operations.

---

## Architecture

```text
┌──────────────────────────────┐
│        Windows PC            │
│                              │
│        PowerShell            │
│             │                │
│             ▼                │
│          AWS CLI             │
│             │                │
│      Profile:                │
│      aws-learning-user       │
└──────────────┬───────────────┘
               │
               ▼
        AWS Authentication
               │
               ▼
        IAM Authorization
          /           \
         /             \
        ▼               ▼
      S3                IAM
   Allowed            Denied
```

---

# Steps Performed

## 1. Verified AWS Identity

The configured CLI profile was tested with:

```powershell
aws sts get-caller-identity --profile aws-learning-user
```

AWS returned the IAM identity associated with the credentials.

This confirmed that the CLI was authenticated as:

```text
aws-learning-user
```

---

## 2. Tested Amazon S3 Access

The following command was executed:

```powershell
aws s3 ls --profile aws-learning-user
```

### Result

The command completed without an error and returned no bucket entries.

This means the command itself was successfully processed, with no S3 buckets being returned in the output.

The test demonstrated that the configured identity could perform the S3 list operation.

---

## 3. Tested IAM List Users

The following command was executed:

```powershell
aws iam list-users --profile aws-learning-user
```

### Result

The command failed with:

```text
AccessDenied
```

AWS reported that:

```text
aws-learning-user
is not authorized to perform:
iam:ListUsers
```

AWS also indicated that no identity-based policy allowed the `iam:ListUsers` action.

### Why?

The current user receives:

```text
AmazonS3ReadOnlyAccess
```

This policy does not provide the required IAM permission:

```text
iam:ListUsers
```

Therefore AWS rejected the request.

---

## 4. Tested IAM Get User

The following command was executed:

```powershell
aws iam get-user --profile aws-learning-user
```

### Result

The command also failed with:

```text
AccessDenied
```

AWS reported that the user was not authorized to perform:

```text
iam:GetUser
```

AWS again indicated that no identity-based policy allowed the requested action.

---

# What I Tested

| CLI Command | Result | Explanation |
|---|---|---|
| `aws sts get-caller-identity` | ✅ Success | AWS identified the authenticated IAM identity |
| `aws s3 ls` | ✅ Success / no bucket output | S3 list operation completed without an error |
| `aws iam list-users` | ❌ AccessDenied | `iam:ListUsers` permission is not available |
| `aws iam get-user` | ❌ AccessDenied | `iam:GetUser` permission is not available |

---

# What Failed

Two IAM commands failed:

```powershell
aws iam list-users --profile aws-learning-user
```

and:

```powershell
aws iam get-user --profile aws-learning-user
```

Both returned `AccessDenied`.

The failures were caused by missing IAM permissions rather than invalid AWS CLI credentials.

---

# How I Fixed It

No permission changes were made.

The `AccessDenied` results were intentionally observed as part of the IAM learning exercise.

The current IAM permissions were kept unchanged because the roadmap contains a dedicated lab for:

```text
L013 — AccessDenied Troubleshooting
```

and later:

```text
L014 — Least-Privilege IAM Policy
```

The goal is to understand why the request was denied before changing permissions.

---

# What I Learned

## 1. AWS CLI uses IAM permissions

The AWS CLI does not bypass IAM.

Every AWS API request is evaluated using AWS security and authorization rules.

---

## 2. Valid credentials do not mean full access

An identity can successfully authenticate but still be denied when attempting an operation for which it has no permission.

```text
Valid credentials
       ↓
Authentication succeeds
       ↓
IAM evaluates permissions
       ↓
Permission exists?
     /       \
   YES        NO
    ↓          ↓
 Allowed    AccessDenied
```

---

## 3. Permissions are action-specific

For example:

```text
iam:ListUsers
```

and:

```text
iam:GetUser
```

are separate IAM actions.

Having permission to use S3 does not automatically grant permission to perform IAM actions.

---

## 4. Groups can provide permissions to users

The permission structure used in this lab was:

```text
User
 ↓
Group
 ↓
Managed Policy
 ↓
Permissions
```

Specifically:

```text
aws-learning-user
       ↓
   Developers
       ↓
AmazonS3ReadOnlyAccess
```

---

# Security Principle

AWS credentials must be kept private.

Never commit the following to GitHub:

```text
AWS Access Key ID
AWS Secret Access Key
AWS CLI credentials
Passwords
API keys
Tokens
Private certificates
```

The AWS CLI profile credentials remain local and are not part of this repository.

---

# SAA Takeaway

A key AWS security concept demonstrated by this lab is:

```text
Authentication
     ↓
"Who are you?"
     ↓
IAM identity
     ↓
Authorization
     ↓
"What are you allowed to do?"
     ↓
IAM policy evaluation
     ↓
Allow / Deny
```

Therefore:

> **Successful authentication does not guarantee authorization.**

An IAM identity can authenticate successfully and still receive `AccessDenied` when the required permission is missing.

---

# Cleanup

No additional AWS infrastructure was created during this lab.

No EC2 instances, databases, load balancers, or other billable infrastructure were created.

The existing IAM configuration was left unchanged for the upcoming IAM labs.

---

# Result

AWS CLI was successfully used to interact with AWS.

The lab demonstrated:

- Successful AWS CLI authentication.
- Successful S3 CLI interaction.
- IAM permission enforcement.
- `AccessDenied` caused by missing IAM permissions.
- The difference between authentication and authorization.

The observed `AccessDenied` behavior will be investigated further in **L013 — AccessDenied Troubleshooting**.