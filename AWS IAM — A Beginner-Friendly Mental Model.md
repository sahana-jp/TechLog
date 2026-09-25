# AWS IAM — A Beginner-Friendly Mental Model

AWS IAM becomes much easier once you understand three core concepts:

* **Users** → Persistent identities, typically for people.
* **Roles** → Temporary identities assumed by AWS services or other trusted entities.
* **Policies** → JSON permission rules defining which actions an identity can perform on which resources.

## The Mental Model

> **Identity (User/Role) + Permissions (Policy) = What can access what**

A **user** or **role** defines *who you are*.
A **policy** defines *what you're allowed to do*.

## Least Privilege

The key security principle is **least privilege**:

> Give every identity only the permissions it needs to do its job — nothing more.

For example, if a Lambda function only needs to read objects from one S3 bucket, it shouldn't receive `AdministratorAccess`.

Instead, grant only the required permission:

```json
{
  "Effect": "Allow",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::my-bucket/*"
}
```

This limits the impact if the Lambda function is ever compromised.

## IAM Checklist

* 🔐 Enable **MFA** on the AWS root user and avoid using root for everyday work.
* 👤 Use a separate identity for normal development.
* 🎭 Use **IAM roles** for services such as Lambda and EC2 instead of long-lived credentials.
* 🔎 Start with managed policies when learning, then narrow permissions toward least privilege.
* 🔑 **Never hardcode AWS access keys** or commit them to Git.
* 🚫 Treat `AccessDenied` as useful information — identify the missing permission instead of granting broad administrative access.

## Key Takeaway

**Users are people. Roles are services. Policies define permissions.**

When you encounter `AccessDenied`, don't immediately reach for `AdministratorAccess`.

Check the policy, identify the permission that's missing, and grant only what's actually required.

The goal isn't to eliminate permission errors by granting everything. It's to understand exactly what access an application needs and follow the **principle of least privilege**.
