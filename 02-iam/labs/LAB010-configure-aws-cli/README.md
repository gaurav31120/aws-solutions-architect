# Lab 010 — Configure AWS CLI

## Objective

Configure AWS CLI on Windows and connect it to AWS using the IAM test user `aws-learning-user`.

The goal of this lab was to:

- Install AWS CLI v2.
- Verify the AWS CLI installation.
- Configure an AWS CLI profile.
- Configure the AWS Region and output format.
- Authenticate the CLI with AWS.
- Verify which AWS identity is being used.

---

## Real-World Story

Imagine a company employee who normally manages AWS through a web browser.

The employee can open:

```text
AWS Console
    ↓
Click IAM
    ↓
Click Users
    ↓
Click other AWS services
```

But developers and DevOps engineers often need to automate these operations.

Instead of clicking through the AWS Console, they can use the AWS Command Line Interface (CLI):

```text
Windows PowerShell
        ↓
     AWS CLI
        ↓
    AWS services
```

The AWS CLI allows us to interact with AWS from the command line.

---

## Concept — What is AWS CLI?

AWS CLI stands for **AWS Command Line Interface**.

It is a command-line tool that allows us to interact with AWS services using commands.

For example:

```powershell
aws sts get-caller-identity
```

Instead of manually navigating through the AWS Console, we can perform many AWS operations through commands.

### Console vs CLI

| AWS Console | AWS CLI |
|---|---|
| Graphical interface | Command-line interface |
| Click buttons | Run commands |
| Good for learning/exploration | Good for automation and repeatable operations |
| Browser based | Terminal based |

---

## Architecture

```text
                    AWS Account
                         │
                         │
                  ┌──────▼──────┐
                  │     IAM     │
                  │             │
                  │ aws-learning│
                  │    -user    │
                  └──────┬──────┘
                         │
                    Credentials
                         │
                         ▼
┌─────────────────────────────────────────┐
│             Windows PC                  │
│                                         │
│             PowerShell                  │
│                  │                      │
│                  ▼                      │
│              AWS CLI v2                 │
│                  │                      │
│                  ▼                      │
│        Profile: aws-learning-user       │
└──────────────────┬──────────────────────┘
                   │
                   ▼
              AWS Services
```

---

## Steps Performed

### 1. Installed AWS CLI

AWS CLI v2 was installed on the Windows machine.

The installation was verified using:

```powershell
aws --version
```

The installed version was:

```text
aws-cli/2.37.4
```

The CLI installation was therefore successful.

---

### 2. Created CLI Access Key

An access key was created for the IAM user:

```text
aws-learning-user
```

The access key was created specifically for command-line access.

The Access Key ID and Secret Access Key were kept private and were not added to GitHub.

---

### 3. Configured a Named AWS CLI Profile

Instead of configuring the default AWS CLI profile, a named profile was created:

```text
aws-learning-user
```

The configuration command used was:

```powershell
aws configure --profile aws-learning-user
```

The configuration included:

```text
Profile:
aws-learning-user

Default region:
ap-south-1

Output format:
json
```

The credentials themselves were entered locally and were not stored in this repository.

---

### 4. Verified AWS Authentication

The configured profile was tested using:

```powershell
aws sts get-caller-identity --profile aws-learning-user
```

AWS returned information containing:

```json
{
    "UserId": "...",
    "Account": "...",
    "Arn": "arn:aws:iam::...:user/aws-learning-user"
}
```

This confirmed that the AWS CLI was successfully authenticated and AWS recognized the configured IAM identity.

---

## Important Configuration

### AWS CLI Profile

```text
aws-learning-user
```

### AWS Region

```text
ap-south-1
```

This is the AWS Mumbai Region.

### Output Format

```text
json
```

JSON is useful for CLI output because it is structured and easy to read and process programmatically.

---

## What I Tested

### Test 1 — AWS CLI Installation

Command:

```powershell
aws --version
```

Result:

```text
AWS CLI v2 installed successfully
```

---

### Test 2 — AWS Identity

Command:

```powershell
aws sts get-caller-identity --profile aws-learning-user
```

Result:

```text
AWS successfully identified the IAM user
aws-learning-user
```

This confirmed that the configured credentials were working.

---

## What Failed

Initially, the `aws` command was not recognized after installation because the current PowerShell session had not picked up the updated PATH.

The terminal was restarted and the command was run again.

After reopening PowerShell:

```powershell
aws --version
```

worked successfully.

---

## How I Fixed It

The PowerShell session was restarted so that the newly installed AWS CLI was available through the updated PATH.

After restarting PowerShell, the CLI version was successfully displayed.

---

## What I Learned

### 1. AWS CLI

AWS CLI allows AWS services to be managed from the command line.

### 2. CLI Profile

A named profile allows different AWS credentials/configurations to be separated.

Example:

```text
aws-learning-user
```

### 3. Region

The CLI can have a default AWS Region configured:

```text
ap-south-1
```

### 4. Authentication

The CLI uses configured credentials to authenticate requests to AWS.

### 5. STS Identity Check

This command:

```powershell
aws sts get-caller-identity --profile aws-learning-user
```

is useful for confirming which AWS identity is being used.

---

## Security Principle

AWS credentials are sensitive.

The following must never be committed to GitHub:

```text
AWS Access Key ID
AWS Secret Access Key
AWS CLI credentials
.env files containing secrets
API keys
Passwords
Tokens
```

The repository `.gitignore` protects sensitive AWS CLI files such as:

```text
.aws/
credentials
config
```

Access keys and secret keys should never be placed inside README files, source code, screenshots, or Git commits.

---

## SAA Takeaway

Remember:

```text
AWS Console
    ↓
Graphical way to interact with AWS

AWS CLI
    ↓
Command-line way to interact with AWS
```

The CLI still follows the same AWS security model.

```text
CLI request
    ↓
Authentication
    ↓
IAM identity
    ↓
IAM permissions
    ↓
AWS service
```

Having valid credentials does **not** automatically mean the identity can perform every AWS operation.

The identity must also have the required IAM permissions.

---

## Cleanup

No AWS compute, database, networking, or storage resources were created by this lab.

The IAM CLI access key created for the learning user is retained for the upcoming IAM CLI exercises.

The credentials must remain private and must never be committed to GitHub.

---

## Result

AWS CLI v2 was successfully installed, configured with the `aws-learning-user` profile, and authenticated against AWS.

The identity was successfully verified using AWS STS.