# IAM User Permissions Lab

A hands-on lab demonstrating how to create IAM users, assign permissions, and test access control using the AWS Management Console, CloudShell, and AWS CLI.

---

## Lab Overview

| Detail | Info |
|--------|------|
| **Course** | Cloud/AWS Fundamentals |
| **Topic** | Identity and Access Management (IAM) |
| **Tools Used** | AWS CloudShell, AWS CLI (Linux), S3, EC2, STS |
| **Objective** | Create an IAM user, attach policies, and test permissions from a local machine |

---

## Repository Structure

```
iam-permissions-lab/
│
├── README.md                   # This file
│
└── screenshots/
    ├── Full_Name_Image_1.png   # CloudShell: create-user + create-access-key output
    ├── Full_Name_Image_2.png   # CloudShell: attach-user-policy + list-attached-user-policies
    ├── Full_Name_Image_3.png   # Local CLI: aws configure --profile lab-user
    ├── Full_Name_Image_4.png   # Local CLI: sts get-caller-identity output
    ├── Full_Name_Image_5.png   # Local CLI: s3 ls (list all buckets)
    ├── Full_Name_Image_6.png   # Local CLI: s3 mb (create 2 buckets) + updated s3 ls
    ├── Full_Name_Image_7.png   # Local CLI: ec2 describe-instances (Access Denied)
    └── Full_Name_Image_8.png   # Local CLI: ec2 describe-instances (success after policy update)
```

> **Note:** Replace `Full_Name` in each filename with your actual first and last name (e.g., `John_Doe_Image_1.png`).

---

## Tasks Completed

### Task 1 — IAM User Setup via AWS CloudShell (50%)

All commands below were run inside **AWS CloudShell** from the AWS Management Console.

**1. Create an IAM user**
```bash
aws iam create-user --user-name lab-user
```

**2. Generate access keys for the user**
```bash
aws iam create-access-key --user-name lab-user
```

**3. Attach the AmazonS3FullAccess managed policy**
```bash
aws iam attach-user-policy \
  --user-name lab-user \
  --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
```

**4. Verify the policy was attached**
```bash
aws iam list-attached-user-policies --user-name lab-user
```

📸 Screenshots: `Image_1` (user + access key creation) and `Image_2` (policy attachment)

---

### Task 2 — AWS CLI from Local Linux Machine (25%)

All commands below were run on a **local Linux machine** with the AWS CLI installed.

**5. Configure a named profile using the new user's credentials**
```bash
aws configure --profile lab-user
```

📸 Screenshot: `Image_3`

**6. Verify the active identity**
```bash
aws sts get-caller-identity --profile lab-user
```

📸 Screenshot: `Image_4`

**7. List all S3 buckets**
```bash
aws s3 ls --profile lab-user
```

📸 Screenshot: `Image_5`

**8. Create two new S3 buckets**
```bash
aws s3 mb s3://yourname-lab-bucket-one --profile lab-user
aws s3 mb s3://yourname-lab-bucket-two --profile lab-user
aws s3 ls --profile lab-user
```

> Replace `yourname` with a unique identifier — S3 bucket names are globally unique.

📸 Screenshot: `Image_6`

**9. Attempt to list EC2 instances (expected: Access Denied)**
```bash
aws ec2 describe-instances --profile lab-user
```

> This fails intentionally — the user has no EC2 permissions at this stage.

📸 Screenshot: `Image_7`

---

### Task 3 — Challenge: Grant EC2 Read Access (25%)

**10. Attach the AmazonEC2ReadOnlyAccess managed policy**
```bash
aws iam attach-user-policy \
  --user-name lab-user \
  --policy-arn arn:aws:iam::aws:policy/AmazonEC2ReadOnlyAccess
```

**11. Confirm both policies are now attached**
```bash
aws iam list-attached-user-policies --user-name lab-user
```

**12. Re-run the EC2 command (now succeeds)**
```bash
aws ec2 describe-instances --profile lab-user
```

📸 Screenshot: `Image_8`

---

## Key Concepts Demonstrated

- **IAM Users** — individual identities with their own credentials
- **Access Keys** — programmatic credentials used with the AWS CLI
- **AWS Managed Policies** — pre-built permission sets maintained by AWS
- **Least Privilege** — granting only the permissions a user needs (S3 only at first, then adding EC2)
- **Named CLI Profiles** — using `--profile` to switch between AWS identities locally
- **STS (Security Token Service)** — used to verify the current calling identity

---

## Screenshot Reference

| File | Task | What It Shows |
|------|------|----------------|
| `Image_1` | Task 1 | `create-user` and `create-access-key` JSON output in CloudShell |
| `Image_2` | Task 1 | `attach-user-policy` and `list-attached-user-policies` output |
| `Image_3` | Task 2 | `aws configure --profile lab-user` with prompts filled in |
| `Image_4` | Task 2 | `sts get-caller-identity` showing UserId, Account, and Arn |
| `Image_5` | Task 2 | `s3 ls` listing existing buckets |
| `Image_6` | Task 2 | `s3 mb` commands creating 2 buckets + updated `s3 ls` |
| `Image_7` | Task 2 | `ec2 describe-instances` returning **Access Denied** |
| `Image_8` | Task 3 | `ec2 describe-instances` returning **successful output** |

---

## Prerequisites

- An active AWS account
- AWS CLI installed on a local Linux machine
- Access to AWS CloudShell via the Management Console
- IAM permissions to create users and attach policies (admin or equivalent)

---

## Author

**Eugene Dokye Anokye**  
Submitted for: IAM User Permissions Lab  
