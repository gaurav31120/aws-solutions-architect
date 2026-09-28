# LAB006 — Create IAM Group

## 🎯 Objective

Create an IAM Group named `Developers`, add the existing IAM User `aws-learning-user` to the group, and understand why IAM Groups are useful for managing users and permissions.

---

## 🧠 Real-World Story

Imagine that GauravMart has a large development team.

The company has many developers:

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
    👤   👤   👤
  Gaurav Priya Rahul

If every developer needs similar AWS permissions, managing each user separately would become difficult.

Instead, we can create a group:

    👥 Developers
          │
      ┌───┼───┐
      │   │   │
      ▼   ▼   ▼
      👤  👤  👤
    Gaurav Priya Rahul

The group allows us to organize users according to their job function.

---

# 💡 What is an IAM Group?

An IAM Group is a collection of IAM Users.

For example:

    👥 Developers
          │
          ├── 👤 aws-learning-user
          ├── 👤 User 2
          └── 👤 User 3

Instead of thinking about every individual user separately, we can manage users as members of a group.

---

# 👤 IAM User vs 👥 IAM Group

An IAM User represents an individual identity.

    👤 IAM USER
         │
         ▼
    aws-learning-user

An IAM Group represents a collection of users.

    👥 IAM GROUP
         │
         ├── 👤 User 1
         ├── 👤 User 2
         └── 👤 User 3

Therefore:

    👤 USER
       =
    Individual identity


    👥 GROUP
       =
    Collection of users

---

# 🔐 Groups and Permissions

IAM Groups become especially useful when managing permissions.

For example:

    👥 Developers
          │
          ▼
      📜 IAM Policy
          │
          ▼
      🔐 Permissions

The developers in the group can receive permissions appropriate for their job function.

For this lab, we intentionally did NOT attach a permissions policy.

We will learn IAM Policies in the next lab.

---

# 🏗️ Architecture

The IAM structure after this lab is:

    🏢 AWS ACCOUNT
          │
          ▼
        🛡️ IAM
          │
          ▼
    👥 Developers
          │
          ▼
    👤 aws-learning-user

Before this lab:

    IAM
     │
     └── 👤 aws-learning-user

After this lab:

    IAM
     │
     └── 👥 Developers
              │
              └── 👤 aws-learning-user

---

# 🧪 Practical Lab

## Step 1 — Open IAM

Open the AWS Management Console.

Search for:

**IAM**

Open:

**IAM — Identity and Access Management**

---

## Step 2 — Open User Groups

From the IAM navigation menu, open:

**User groups**

---

## Step 3 — Create the Group

Click:

**Create group**

For the group name, enter:

**Developers**

The group represents the development team in our GauravMart example.

---

## Step 4 — Add User

AWS provided an option to add users to the group.

The existing IAM user was:

**aws-learning-user**

Select:

**aws-learning-user**

This creates the relationship:

    👥 Developers
          │
          ▼
    👤 aws-learning-user

---

## Step 5 — Attach Permissions Policies

AWS also provided an option to attach permissions policies.

For this lab:

**No policy was selected.**

The reason is that this lab is focused on understanding IAM Groups.

IAM Policies will be studied separately.

---

## Step 6 — Create the Group

After selecting the user and leaving policies unselected, create the group.

AWS successfully created:

**Developers**

---

# 🔍 Verification

After creating the group:

1. Opened **IAM**.
2. Opened **User groups**.
3. Opened the **Developers** group.
4. Checked the users in the group.
5. Confirmed that:

**aws-learning-user**

is a member of:

**Developers**

Final structure:

    👥 Developers
          │
          ▼
    👤 aws-learning-user

The group and membership were successfully verified.

---

# ⚙️ Important Configuration

### Group Name

**Developers**

### User Added

**aws-learning-user**

### Policies Attached

**None**

### Permission Boundary

**None**

---

# 🧪 What I Tested

### Test 1 — Group creation

Created the IAM Group:

**Developers**

Result:

**Successful**

### Test 2 — User membership

Added:

**aws-learning-user**

to:

**Developers**

Result:

**Successful**

### Test 3 — Verification

Opened the Developers group and verified that the user appears as a group member.

Result:

**Successful**

---

# ❌ What Failed

Nothing failed during this lab.

The group was created successfully and the IAM user was successfully added.

---

# 🔧 How I Fixed It

No troubleshooting was required for this lab.

---

# 🧠 What I Learned

- An IAM User represents an individual identity.
- An IAM Group is a collection of IAM Users.
- Users can be added to groups.
- Groups are useful for organizing users by job function.
- Groups can help simplify permission management.
- A group can contain multiple users.
- We did not attach policies in this lab.
- IAM Policies will determine permissions in the next lab.
- Identity and permissions are separate concepts.

---

# 🔐 Security Principle

Groups help organize permissions, but permissions should still follow the principle of least privilege.

The goal is:

    🔐 LEAST PRIVILEGE
           │
           ▼
    Give users only the
    permissions they actually need.

Do not give broad administrative permissions unnecessarily.

---

# 🎯 SAA Takeaway

Remember:

    👤 IAM User
         =
    Individual identity


    👥 IAM Group
         =
    Collection of users


    📜 IAM Policy
         =
    Defines permissions

The basic relationship is:

    👥 GROUP
         │
         ├── 👤 USER
         ├── 👤 USER
         └── 👤 USER

Groups are useful for managing users with similar permission requirements.

---

# 💼 Real-World Developer Example

Imagine a company has:

    👥 Developers
         │
         ├── 👤 Backend Developer
         ├── 👤 Frontend Developer
         └── 👤 DevOps Developer

Instead of managing every developer independently, the company can organize users into appropriate groups and manage permissions according to job responsibilities.

This becomes especially useful as the number of users grows.

---

# 🧹 Cleanup

For this lab, no additional AWS resources were created that generate significant ongoing infrastructure costs.

The IAM Group and User are intentionally retained because they will be used in subsequent IAM labs.

Do not delete them yet.

The next lab will build on this setup.

---

# 📌 Lab Status

**LAB006 — ✅ COMPLETED**

Created IAM Group:

**Developers**

Added IAM User:

**aws-learning-user**

---

# ⏭️ Next Lab

**LAB007 — Attach Managed Policy**

In the next lab, we will finally answer:

> What permissions should our `Developers` group have?

We will introduce:

    👥 Developers
          │
          ▼
      📜 IAM Policy
          │
          ▼
      🔐 Permissions

---