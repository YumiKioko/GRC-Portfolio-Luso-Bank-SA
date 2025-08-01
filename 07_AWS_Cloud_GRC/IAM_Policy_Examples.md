# 🛡️ IAM Policy Examples – LusoBank S.A.

This document contains examples of AWS IAM policies and Service Control Policies (SCPs) designed for a secure cloud environment in a financial institution context.

---

## 📄 Read-Only Auditor Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*",
        "s3:ListAllMyBuckets",
        "s3:GetBucketLocation",
        "s3:ListBucket",
        "s3:GetObject",
        "iam:List*",
        "cloudtrail:LookupEvents",
        "cloudwatch:Describe*",
        "logs:Describe*",
        "config:Describe*"
      ],
      "Resource": "*"
    }
  ]
}
````

---

## 🔐 Least Privilege S3 Access

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "S3ReadAccess",
      "Effect": "Allow",
      "Action": ["s3:GetObject"],
      "Resource": ["arn:aws:s3:::lusobank-client-data/*"]
    }
  ]
}
```

---

## 🚫 SCP Example – Deny Dangerous IAM Actions

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyIAMPassRole",
      "Effect": "Deny",
      "Action": [
        "iam:PassRole",
        "iam:CreatePolicyVersion",
        "iam:SetDefaultPolicyVersion",
        "iam:AttachRolePolicy",
        "iam:PutRolePolicy"
      ],
      "Resource": "*"
    }
  ]
}
```

---

## 📍 SCP Example – Restrict AWS Region Use

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyUnsupportedRegions",
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:RequestedRegion": ["eu-west-1", "eu-central-1"]
        }
      }
    }
  ]
}
```

---

## ✅ Notes

* IAM policies should be reviewed quarterly as part of access governance.
* SCPs apply at the AWS Organization root or OU level.

---

## 🔎 Summary & Recommendations

### Read-Only Auditor Policy

* Grants read-only access across critical AWS services.
* ✅ Uses `Describe`, `List`, `Get` actions only.
* 💡 Consider modularizing by service type for easier auditability.

### Least Privilege S3 Access

* Tight access to a specific bucket.
* ✅ Follows least privilege principles.
* 💡 Add conditions for IP address, MFA, or object prefixes if required.

### SCP – Deny Dangerous IAM Actions

* Blocks high-risk actions like `PassRole` and policy versioning.
* ✅ Great for controlling privilege escalation.
* 💡 Optionally add: `iam:UpdateAssumeRolePolicy`, `iam:CreateRole`.

### SCP – Region Restrictions

* Denies actions outside approved AWS regions.
* ✅ Protects data residency and compliance.
* 💡 Consider including a list of explicitly allowed services if needed.

---

Let me know when you're ready for the next file:
**`AWS_Compliance_Playbook_LusoBank.docx`** or the **README update**.
