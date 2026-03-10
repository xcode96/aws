# 📋 COMPLETE AWS CLI COMMANDS - EVERY COMMAND WITH COLOR-CODED EXPLANATIONS!

## 🎨 COLOR CODE:
- 🔴 **RED** = CRITICAL/VERY IMPORTANT (Exploit, Danger, Flag)
- 🟠 **ORANGE** = Important Actions (Creating, Changing)
- 🟡 **YELLOW** = Information to Look For (Usernames, IDs, Keys)
- 🟢 **GREEN** = Success/Good Output
- 🔵 **BLUE** = Commands You Type

---

# PART 1: AWS CONFIGURATION COMMANDS

## 🔵 `aws configure`
**WHY:** First command! Sets up your AWS access

```
🔴 AWS Access Key ID [None]: AKIAQ3EGUZME3BAXFV7D
🔴 AWS Secret Access Key [None]: 2ETmQExURN6kBI0/2kClgcuTLyVBD5idkrJSFvsY
🔴 Default region name [None]: us-east-1
🔴 Default output format [None]: json
```

## 🔵 `aws configure list`
**WHY:** Check current settings

```
      Name                    Value             Type    Location
      ----                    -----             ----    --------
   profile                <not set>             None    None
🔵 access_key     ****************FV7D shared-credentials-file
🔵 secret_key     ****************FvsY shared-credentials-file
🔵 region                us-east-1      config-file    ~/.aws/config
```

## 🔵 `aws configure get region`
**WHY:** Get current region

```
🔵 us-east-1
```

## 🔵 `aws configure set region us-west-2`
**WHY:** Change region

## 🔵 `aws configure --profile prod`
**WHY:** Create separate profile for production

```
🔴 AWS Access Key ID [None]: AKIAPRODKEY123456
🔴 AWS Secret Access Key [None]: ProdSecretKey123456
```

## 🔵 `aws configure list-profiles`
**WHY:** See all profiles

```
default
prod
dev
```

---

# PART 2: STS (SECURITY TOKEN SERVICE) COMMANDS

## 🔵 `aws sts get-caller-identity`
**WHY:** 🔴 **VERIFY WHO YOU ARE!** (First thing to check)

```json
{
    "UserId": "AIDAQ3EGUZME24ILVB44T",
    "Account": "058264439561",
    🟡 "Arn": "arn:aws:iam::058264439561:user/BackupReader1"  <-- 🔴 LOOK HERE!
}
```

## 🔵 `aws sts assume-role --role-arn "arn:aws:iam::123456789012:role/AdminRole" --role-session-name "MySession"`
**WHY:** 🔴 Get temporary admin access

```json
{
    "Credentials": {
        🟡 "AccessKeyId": "ASIAIOSFODNN7EXAMPLE",
        🟡 "SecretAccessKey": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYzEXAMPLEKEY",
        🟡 "SessionToken": "IQoJb3JpZ2luX2VjEIX...",
        🟡 "Expiration": "2025-03-10T23:18:00Z"
    }
}
```

## 🔵 `aws sts get-session-token --duration-seconds 3600`
**WHY:** Get temporary credentials

```json
{
    "Credentials": {
        🟡 "AccessKeyId": "ASIAIOSFODNN7EXAMPLE",
        🟡 "SecretAccessKey": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYzEXAMPLEKEY",
        🟡 "SessionToken": "FwoGZXIvYXdzEHca...",
        🟡 "Expiration": "2025-03-10T22:30:00Z"
    }
}
```

## 🔵 `aws sts decode-authorization-message --encoded-message encoded-message-string`
**WHY:** Understand why you got "Access Denied"

```json
{
    "DecodedMessage": "{
        🟡 \"allowed\": false,
        🟡 \"missing permission\": \"s3:PutObject\"
    }"
}
```

---

# PART 3: IAM (IDENTITY AND ACCESS MANAGEMENT) - ALL COMMANDS!

## 🔵 USER COMMANDS

### 🔵 `aws iam list-users`
**WHY:** See all users in account

```json
{
    "Users": [
        {
            🟡 "UserName": "BackupReader1",
            🟡 "UserId": "AIDAQ3EGUZME24ILVB44T",
            🟡 "Arn": "arn:aws:iam::058264439561:user/BackupReader1",
            🟡 "CreateDate": "2024-09-24T11:12:45+00:00"
        },
        {
            🟡 "UserName": "AdminUser",
            🟡 "UserId": "AIDAQ3EGUZME5HY78JKL"
        }
    ]
}
```

### 🔵 `aws iam get-user --user-name BackupReader1`
**WHY:** 🔴 Get detailed user info

```json
{
    "User": {
        🟡 "UserName": "BackupReader1",
        🟡 "UserId": "AIDAQ3EGUZME24ILVB44T",
        🟡 "Arn": "arn:aws:iam::058264439561:user/BackupReader1",
        "PermissionsBoundary": {
            🔴 "PermissionsBoundaryArn": "arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy",
            🔴 "PermissionsBoundaryType": "Policy"
        }
    }
}
```

### 🔵 `aws iam get-user --query "User.PermissionsBoundary" --output text`
**WHY:** 🔴 QUICKLY find your restrictions

```
🔴 arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy       Policy
```

### 🔵 `aws iam create-user --user-name NewUser`
**WHY:** Create new user

```json
{
    "User": {
        🟡 "UserName": "NewUser",
        🟡 "UserId": "AIDAQ3EGUZME12345678",
        🟡 "Arn": "arn:aws:iam::058264439561:user/NewUser"
    }
}
```

### 🔵 `aws iam update-user --user-name OldName --new-user-name NewName`
**WHY:** Rename user

```json
{
    "User": {
        🟡 "UserName": "NewName",
        "UserId": "AIDAQ3EGUZME12345678"
    }
}
```

### 🔵 `aws iam delete-user --user-name UserToDelete`
**WHY:** 🔴 Permanently delete user (careful!)

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws iam create-login-profile --user-name BackupReader1 --password TempPass123! --password-reset-required`
**WHY:** Enable console access with password

```json
{
    "LoginProfile": {
        🟡 "UserName": "BackupReader1",
        🟡 "CreateDate": "2025-03-10T15:30:22+00:00"
    }
}
```

### 🔵 `aws iam update-login-profile --user-name BackupReader1 --password NewPass456!`
**WHY:** Change user password

```json
🟢 (No output = SUCCESS!)
```

### 🔵 `aws iam delete-login-profile --user-name BackupReader1`
**WHY:** Remove console access

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 GROUP COMMANDS

### 🔵 `aws iam list-groups`
**WHY:** See all groups

```json
{
    "Groups": [
        {
            🟡 "GroupName": "Admins",
            🟡 "GroupId": "AGPAQ3EGUZME4X5Y6Z7A",
            🟡 "Arn": "arn:aws:iam::058264439561:group/Admins"
        },
        {
            🟡 "GroupName": "Developers",
            🟡 "GroupId": "AGPAQ3EGUZME8B9C0D1E"
        }
    ]
}
```

### 🔵 `aws iam get-group --group-name Developers`
**WHY:** See users in a group

```json
{
    "Group": {
        🟡 "GroupName": "Developers",
        🟡 "GroupId": "AGPAQ3EGUZME8B9C0D1E"
    },
    "Users": [
        {
            🟡 "UserName": "dev1",
            🟡 "UserId": "AIDAQ3EGUZME12345"
        }
    ]
}
```

### 🔵 `aws iam create-group --group-name NewGroup`
**WHY:** Create new group

```json
{
    "Group": {
        🟡 "GroupName": "NewGroup",
        🟡 "GroupId": "AGPAQ3EGUZME9876543",
        🟡 "Arn": "arn:aws:iam::058264439561:group/NewGroup"
    }
}
```

### 🔵 `aws iam delete-group --group-name NewGroup`
**WHY:** Delete group

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws iam add-user-to-group --user-name BackupReader1 --group-name Developers`
**WHY:** Add user to group

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws iam remove-user-from-group --user-name BackupReader1 --group-name Developers`
**WHY:** Remove user from group

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws iam list-groups-for-user --user-name BackupReader1`
**WHY:** See what groups user is in

```json
{
    "Groups": [
        {
            🟡 "GroupName": "Developers",
            🟡 "GroupId": "AGPAQ3EGUZME8B9C0D1E"
        }
    ]
}
```

---

## 🔵 ROLE COMMANDS

### 🔵 `aws iam list-roles`
**WHY:** See all IAM roles

```json
{
    "Roles": [
        {
            🟡 "RoleName": "LambdaExecutionRole",
            🟡 "RoleId": "AROAQ3EGUZME12345",
            🟡 "Arn": "arn:aws:iam::058264439561:role/LambdaExecutionRole"
        },
        {
            🟡 "RoleName": "EC2InstanceRole",
            🟡 "RoleId": "AROAQ3EGUZME67890"
        }
    ]
}
```

### 🔵 `aws iam get-role --role-name LambdaExecutionRole`
**WHY:** Get role details

```json
{
    "Role": {
        🟡 "RoleName": "LambdaExecutionRole",
        🟡 "RoleId": "AROAQ3EGUZME12345",
        🟡 "Arn": "arn:aws:iam::058264439561:role/LambdaExecutionRole",
        "AssumeRolePolicyDocument": {
            "Statement": [
                {
                    🟡 "Principal": {
                        "Service": "lambda.amazonaws.com"
                    },
                    "Action": "sts:AssumeRole"
                }
            ]
        }
    }
}
```

### 🔵 `aws iam create-role --role-name NewRole --assume-role-policy-document file://trust-policy.json`
**WHY:** Create new role

```json
{
    "Role": {
        🟡 "RoleName": "NewRole",
        🟡 "RoleId": "AROAQ3EGUZME54321",
        🟡 "Arn": "arn:aws:iam::058264439561:role/NewRole"
    }
}
```

### 🔵 `aws iam delete-role --role-name NewRole`
**WHY:** 🔴 Delete role

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws iam update-assume-role-policy --role-name NewRole --policy-document file://new-trust.json`
**WHY:** Change who can assume the role

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 POLICY COMMANDS - 🔴 **MOST IMPORTANT FOR LAB!**

### 🔵 `aws iam list-policies`
**WHY:** See all policies

```json
{
    "Policies": [
        {
            🟡 "PolicyName": "PermissionBoundaryPolicy",
            🟡 "PolicyId": "ANPAQ3EGUZMEQVY32A5L4",
            🔴 "Arn": "arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy",
            🔴 "DefaultVersionId": "v3"
        },
        {
            🟡 "PolicyName": "UserPolicy",
            🟡 "PolicyId": "ANPAQ3EGUZMEYBBY6PGTS",
            🔴 "Arn": "arn:aws:iam::058264439561:policy/UserPolicy",
            🔴 "DefaultVersionId": "v1"
        }
    ]
}
```

### 🔵 `aws iam get-policy --policy-arn arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy`
**WHY:** 🔴 Get policy details to find version

```json
{
    "Policy": {
        🟡 "PolicyName": "PermissionBoundaryPolicy",
        🔴 "DefaultVersionId": "v3"  <-- IMPORTANT!
    }
}
```

### 🔵 `aws iam get-policy-version --policy-arn arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy --version-id v3`
**WHY:** 🔴 **THIS SHOWS YOUR ACTUAL PERMISSIONS!** (CRITICAL!)

```json
{
    "PolicyVersion": {
        "Document": {
            "Statement": [
                {
                    "Action": [
                        🔴 "s3:ListBucket",      <-- YOU CAN LIST BUCKETS
                        🔴 "s3:GetObject"         <-- YOU CAN DOWNLOAD FILES
                    ],
                    "Effect": "Allow",
                    "Resource": [
                        🔴 "arn:aws:s3:::securecopbakupbuk1",
                        🔴 "arn:aws:s3:::securecopbakupbuk1/*"
                    ]
                },
                {
                    "Action": [
                        🔴 "kms:Decrypt"          <-- YOU CAN DECRYPT WITH KMS
                    ],
                    "Effect": "Allow",
                    "Resource": 🔴 "arn:aws:kms:us-east-1:058264439561:key/a7a251b3-c889-4dd1-9176-931c207c33d5"
                },
                {
                    "Action": [
                        🔴 "iam:PutUserPolicy"    <-- 🔴🔴🔴 YOU CAN ATTACH POLICIES TO YOURSELF! (EXPLOIT!)
                    ],
                    "Effect": "Allow",
                    "Resource": 🔴 "arn:aws:iam::058264439561:user/BackupReader1"
                }
            ]
        }
    }
}
```

### 🔵 `aws iam create-policy --policy-name MyNewPolicy --policy-document file://policy.json`
**WHY:** Create a new policy

```json
{
    "Policy": {
        🟡 "PolicyName": "MyNewPolicy",
        🔴 "Arn": "arn:aws:iam::058264439561:policy/MyNewPolicy",
        "DefaultVersionId": "v1"
    }
}
```

### 🔵 `aws iam delete-policy --policy-arn arn:aws:iam::058264439561:policy/MyNewPolicy`
**WHY:** Delete policy

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws iam create-policy-version --policy-arn arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy --policy-document file://new-version.json --set-as-default`
**WHY:** Update policy with new version

```json
{
    "PolicyVersion": {
        🔴 "VersionId": "v4",
        "IsDefaultVersion": true
    }
}
```

---

## 🔵 ATTACHED POLICIES COMMANDS

### 🔵 `aws iam list-attached-user-policies --user-name BackupReader1`
**WHY:** 🔴 See what policies are attached to user

```json
{
    "AttachedPolicies": [
        {
            🟡 "PolicyName": "UserPolicy",
            🔴 "PolicyArn": "arn:aws:iam::058264439561:policy/UserPolicy"
        }
    ]
}
```

### 🔵 `aws iam attach-user-policy --user-name BackupReader1 --policy-arn arn:aws:iam::058264439561:policy/UserPolicy`
**WHY:** Attach policy to user

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws iam detach-user-policy --user-name BackupReader1 --policy-arn arn:aws:iam::058264439561:policy/UserPolicy`
**WHY:** Remove policy from user

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws iam list-attached-group-policies --group-name Developers`
**WHY:** See policies attached to group

### 🔵 `aws iam attach-group-policy --group-name Developers --policy-arn arn:aws:iam::058264439561:policy/UserPolicy`
**WHY:** Attach policy to group

### 🔵 `aws iam detach-group-policy --group-name Developers --policy-arn arn:aws:iam::058264439561:policy/UserPolicy`
**WHY:** Remove policy from group

### 🔵 `aws iam list-attached-role-policies --role-name LambdaExecutionRole`
**WHY:** See policies attached to role

### 🔵 `aws iam attach-role-policy --role-name LambdaExecutionRole --policy-arn arn:aws:iam::058264439561:policy/LambdaPolicy`
**WHY:** Attach policy to role

### 🔵 `aws iam detach-role-policy --role-name LambdaExecutionRole --policy-arn arn:aws:iam::058264439561:policy/LambdaPolicy`
**WHY:** Remove policy from role

---

## 🔵 INLINE POLICY COMMANDS

### 🔵 `aws iam put-user-policy --user-name BackupReader1 --policy-name KMSFullAccessPolicy --policy-document file://policy.json`
**WHY:** 🔴 **PRIVILEGE ESCALATION!** Add inline policy to user

```
🟢 (No output = SUCCESS! You now have more permissions!)
```

### 🔵 `aws iam list-user-policies --user-name BackupReader1`
**WHY:** See inline policies attached to user

```json
{
    "PolicyNames": [
        🔴 "KMSFullAccessPolicy"  <-- Your new policy!
    ]
}
```

### 🔵 `aws iam get-user-policy --user-name BackupReader1 --policy-name KMSFullAccessPolicy`
**WHY:** 🔴 See what permissions your inline policy gives

```json
{
    "UserName": "BackupReader1",
    "PolicyName": "KMSFullAccessPolicy",
    "PolicyDocument": {
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    🔴 "s3:*",     <-- Full S3 access!
                    🔴 "kms:*"      <-- Full KMS access!
                ],
                "Resource": [
                    🔴 "arn:aws:s3:::securecopbakupbuk1",
                    🔴 "arn:aws:s3:::securecopbakupbuk1/*",
                    🔴 "arn:aws:kms:us-east-1:058264439561:key/a7a251b3-c889-4dd1-9176-931c207c33d5"
                ]
            }
        ]
    }
}
```

### 🔵 `aws iam delete-user-policy --user-name BackupReader1 --policy-name KMSFullAccessPolicy`
**WHY:** Remove inline policy

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws iam put-group-policy --group-name Developers --policy-name GroupPolicy --policy-document file://policy.json`
**WHY:** Add inline policy to group

### 🔵 `aws iam list-group-policies --group-name Developers`
**WHY:** List group inline policies

### 🔵 `aws iam get-group-policy --group-name Developers --policy-name GroupPolicy`
**WHY:** Get group inline policy details

### 🔵 `aws iam put-role-policy --role-name LambdaExecutionRole --policy-name RolePolicy --policy-document file://policy.json`
**WHY:** Add inline policy to role

### 🔵 `aws iam list-role-policies --role-name LambdaExecutionRole`
**WHY:** List role inline policies

### 🔵 `aws iam get-role-policy --role-name LambdaExecutionRole --policy-name RolePolicy`
**WHY:** Get role inline policy details

---

## 🔵 ACCESS KEY COMMANDS

### 🔵 `aws iam create-access-key --user-name BackupReader1`
**WHY:** 🔴 Create new access key (for CLI access)

```json
{
    "AccessKey": {
        "UserName": "BackupReader1",
        🔴 "AccessKeyId": "AKIAQ3EGUZME3NEWKEY12",
        🔴 "SecretAccessKey": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYzEXAMPLEKEY",
        "Status": "Active",
        "CreateDate": "2025-03-10T10:25:33Z"
    }
}
```

### 🔵 `aws iam list-access-keys --user-name BackupReader1`
**WHY:** See all access keys for user

```json
{
    "AccessKeyMetadata": [
        {
            "UserName": "BackupReader1",
            🔴 "AccessKeyId": "AKIAQ3EGUZME3BAXFV7D",
            "Status": "Active",
            "CreateDate": "2024-09-24T11:14:23Z"
        }
    ]
}
```

### 🔵 `aws iam update-access-key --user-name BackupReader1 --access-key-id AKIAQ3EGUZME3BAXFV7D --status Inactive`
**WHY:** Disable access key

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws iam delete-access-key --user-name BackupReader1 --access-key-id AKIAQ3EGUZME3BAXFV7D`
**WHY:** 🔴 Delete access key permanently

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 SSH KEY COMMANDS

### 🔵 `aws iam list-ssh-public-keys --user-name BackupReader1`
**WHY:** List SSH public keys for CodeCommit

### 🔵 `aws iam upload-ssh-public-key --user-name BackupReader1 --ssh-public-key-body "ssh-rsa AAAAB3NzaC1y..."`
**WHY:** Upload SSH key for Git access

### 🔵 `aws iam delete-ssh-public-key --user-name BackupReader1 --ssh-public-key-id KEY12345678`
**WHY:** Delete SSH key

---

## 🔵 MFA COMMANDS

### 🔵 `aws iam list-mfa-devices --user-name BackupReader1`
**WHY:** Check if user has MFA enabled

```json
{
    "MFADevices": [
        {
            "UserName": "BackupReader1",
            🔴 "SerialNumber": "arn:aws:iam::058264439561:mfa/BackupReader1"
        }
    ]
}
```

### 🔵 `aws iam enable-mfa-device --user-name BackupReader1 --serial-number arn:aws:iam::058264439561:mfa/BackupReader1 --authentication-code1 123456 --authentication-code2 789012`
**WHY:** Enable MFA for user

### 🔵 `aws iam deactivate-mfa-device --user-name BackupReader1 --serial-number arn:aws:iam::058264439561:mfa/BackupReader1`
**WHY:** 🔴 Disable MFA (security risk!)

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 ACCOUNT SUMMARY COMMANDS

### 🔵 `aws iam get-account-summary`
**WHY:** Get summary of IAM usage

```json
{
    "SummaryMap": {
        🔴 "Users": 15,
        🔴 "Groups": 5,
        🔴 "Roles": 12,
        🔴 "Policies": 25,
        🔴 "AccountMFAEnabled": 1
    }
}
```

### 🔵 `aws iam get-account-authorization-details`
**WHY:** 🔴 Get EVERYTHING about IAM (huge output!)

```json
{
    "UserDetailList": [...],
    "GroupDetailList": [...],
    "RoleDetailList": [...],
    "Policies": [...]
}
```

### 🔵 `aws iam get-account-password-policy`
**WHY:** Check password requirements

```json
{
    "PasswordPolicy": {
        🔴 "MinimumPasswordLength": 8,
        "RequireSymbols": true,
        "RequireNumbers": true
    }
}
```

### 🔵 `aws iam update-account-password-policy --minimum-password-length 12`
**WHY:** Change password policy

---

# PART 4: S3 (SIMPLE STORAGE SERVICE) - HIGH LEVEL COMMANDS

## 🔵 BUCKET OPERATIONS

### 🔵 `aws s3 ls`
**WHY:** List all buckets

```
2024-09-24 11:15:22 🔴 securecopbakupbuk1
2024-01-10 09:30:45 my-app-logs-2025
2024-03-18 14:22:33 data-warehouse-prod
```

### 🔵 `aws s3 mb s3://my-new-bucket`
**WHY:** Make (create) new bucket

```
make_bucket: my-new-bucket
```

### 🔵 `aws s3 rb s3://my-new-bucket`
**WHY:** Remove (delete) empty bucket

```
remove_bucket: my-new-bucket
```

### 🔵 `aws s3 rb s3://my-new-bucket --force`
**WHY:** 🔴 Force delete bucket with files

```
delete: s3://my-new-bucket/file1.txt
delete: s3://my-new-bucket/file2.txt
remove_bucket: my-new-bucket
```

### 🔵 `aws s3 website s3://my-bucket/ --index-document index.html --error-document error.html`
**WHY:** Configure bucket for static website hosting

---

## 🔵 OBJECT OPERATIONS

### 🔵 `aws s3 ls s3://securecopbakupbuk1/`
**WHY:** 🔴 See what files are in bucket

```
                           PRE .BurpSuite/
2024-09-24 16:49:10        411 🔴 Hospital+Patient+Records.zip
2024-09-24 16:48:40        411 🔴 env.txt
2024-09-24 16:43:42        113 🔴 key.txt
```

### 🔵 `aws s3 ls s3://securecopbakupbuk1/ --recursive`
**WHY:** List all files including subfolders

```
2024-09-24 16:43:42        113 key.txt
2024-09-24 16:48:40        411 env.txt
2024-09-24 16:49:10        411 Hospital+Patient+Records.zip
2024-09-24 16:50:22       1228 .BurpSuite/config.json
```

### 🔵 `aws s3 ls s3://securecopbakupbuk1/ --human-readable --summarize`
**WHY:** List with file sizes in human format

```
2024-09-24 16:43:42  113 Bytes key.txt
2024-09-24 16:48:40  411 Bytes env.txt
...
Total Objects: 4
   Total Size: 2.1 KiB
```

### 🔵 `aws s3 cp file.txt s3://securecopbakupbuk1/`
**WHY:** Upload file to bucket

```
upload: .\file.txt to s3://securecopbakupbuk1/file.txt
```

### 🔵 `aws s3 cp s3://securecopbakupbuk1/key.txt .`
**WHY:** 🔴 Download file from bucket

```
🟢 download: s3://securecopbakupbuk1/key.txt to .\key.txt
```

### 🔵 `aws s3 cp s3://securecopbakupbuk1/key.txt s3://securecopbakupbuk1/key-backup.txt`
**WHY:** Copy file within bucket

```
copy: s3://securecopbakupbuk1/key.txt to s3://securecopbakupbuk1/key-backup.txt
```

### 🔵 `aws s3 mv file.txt s3://securecopbakupbuk1/`
**WHY:** Move/upload and delete local file

```
move: .\file.txt to s3://securecopbakupbuk1/file.txt
```

### 🔵 `aws s3 mv s3://securecopbakupbuk1/old.txt s3://securecopbakupbuk1/new.txt`
**WHY:** Rename file in bucket

```
move: s3://securecopbakupbuk1/old.txt to s3://securecopbakupbuk1/new.txt
```

### 🔵 `aws s3 rm s3://securecopbakupbuk1/file.txt`
**WHY:** Delete file from bucket

```
delete: s3://securecopbakupbuk1/file.txt
```

### 🔵 `aws s3 rm s3://securecopbakupbuk1/ --recursive`
**WHY:** 🔴 Delete ALL files in bucket

```
delete: s3://securecopbakupbuk1/key.txt
delete: s3://securecopbakupbuk1/env.txt
delete: s3://securecopbakupbuk1/Hospital+Patient+Records.zip
```

### 🔵 `aws s3 sync local-folder/ s3://securecopbakupbuk1/`
**WHY:** Sync local folder to bucket (upload only new/changed)

```
upload: local-folder\newfile.txt to s3://securecopbakupbuk1/newfile.txt
```

### 🔵 `aws s3 sync s3://securecopbakupbuk1/ downloads/`
**WHY:** Sync bucket to local folder (download only new/changed)

```
download: s3://securecopbakupbuk1/key.txt to downloads\key.txt
```

---

# PART 5: S3API (S3 API-LEVEL COMMANDS) - DETAILED CONTROL

## 🔵 BUCKET OPERATIONS

### 🔵 `aws s3api list-buckets`
**WHY:** List all buckets with details

```json
{
    "Buckets": [
        {
            🔴 "Name": "securecopbakupbuk1",
            "CreationDate": "2024-09-24T11:15:22+00:00"
        }
    ],
    "Owner": {
        "ID": "bcaf1ffd86f41161ca5fb16fd081034f"
    }
}
```

### 🔵 `aws s3api create-bucket --bucket my-api-bucket-123 --region us-east-1`
**WHY:** Create bucket with API

```json
{
    🔴 "Location": "/my-api-bucket-123"
}
```

### 🔵 `aws s3api delete-bucket --bucket my-api-bucket-123`
**WHY:** Delete empty bucket

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws s3api list-objects --bucket securecopbakupbuk1`
**WHY:** 🔴 List objects with metadata

```json
{
    "Contents": [
        {
            🔴 "Key": "key.txt",
            "LastModified": "2024-09-24T16:43:42+00:00",
            🔴 "Size": 113,
            "StorageClass": "STANDARD"
        },
        {
            🔴 "Key": "env.txt",
            "LastModified": "2024-09-24T16:48:40+00:00",
            🔴 "Size": 411
        }
    ]
}
```

### 🔵 `aws s3api list-objects-v2 --bucket securecopbakupbuk1 --max-items 2`
**WHY:** List objects (newer version with pagination)

```json
{
    "Contents": [
        {"Key": ".BurpSuite/", "Size": 0},
        {"Key": "Hospital+Patient+Records.zip", "Size": 411}
    ],
    "KeyCount": 2
}
```

### 🔵 `aws s3api head-bucket --bucket securecopbakupbuk1`
**WHY:** Check if bucket exists and you have access

```json
🟢 (No output = SUCCESS!)
```

---

## 🔵 OBJECT OPERATIONS

### 🔵 `aws s3api put-object --bucket securecopbakupbuk1 --key hello.txt --body hello.txt`
**WHY:** Upload object

```json
{
    🔴 "ETag": "\"5d41402abc4b2a76b9719d911017c592\""
}
```

### 🔵 `aws s3api get-object --bucket securecopbakupbuk1 --key env.txt env-output.txt`
**WHY:** Download object

```json
{
    "AcceptRanges": "bytes",
    "LastModified": "2024-09-24T16:48:40+00:00",
    🔴 "ContentLength": 411,
    "ContentType": "binary/octet-stream"
}
```

### 🔵 `aws s3api get-object --bucket securecopbakupbuk1 --key env.txt ./env9.txt --sse-customer-algorithm AES256 --sse-customer-key ZhL8Zvaa3Xybf/sizoGwtrgKpxkxE46JJMiz1syVGQM= --sse-customer-key-md5 ESUzbd203tahLpxvA6LL4g==`
**WHY:** 🔴 **DOWNLOAD AND DECRYPT WITH CUSTOMER KEY!** (LAB EXPLOIT!)

```json
{
    "AcceptRanges": "bytes",
    "LastModified": "2024-09-24T16:48:40+00:00",
    🔴 "ContentLength": 411,
    🟢 "SSECustomerAlgorithm": "AES256",              <-- DECRYPTED!
    🟢 "SSECustomerKeyMD5": "ESUzbd203tahLpxvA6LL4g=="
}
```

### 🔵 `aws s3api delete-object --bucket securecopbakupbuk1 --key hello.txt`
**WHY:** Delete object

```json
{
    "DeleteMarker": true
}
```

### 🔵 `aws s3api copy-object --bucket securecopbakupbuk1 --copy-source securecopbakupbuk1/key.txt --key key-copy.txt`
**WHY:** Copy object

```json
{
    "CopyObjectResult": {
        "ETag": "\"d41d8cd98f00b204e9800998ecf8427e\"",
        "LastModified": "2025-03-10T11:30:45+00:00"
    }
}
```

### 🔵 `aws s3api head-object --bucket securecopbakupbuk1 --key key.txt`
**WHY:** Get object metadata without downloading

```json
{
    "AcceptRanges": "bytes",
    "LastModified": "2024-09-24T16:43:42+00:00",
    🔴 "ContentLength": 113,
    🔴 "ETag": "\"d41d8cd98f00b204e9800998ecf8427e\""
}
```

---

## 🔵 BUCKET POLICY COMMANDS

### 🔵 `aws s3api put-bucket-policy --bucket securecopbakupbuk1 --policy file://policy.json`
**WHY:** Set bucket permissions

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws s3api get-bucket-policy --bucket securecopbakupbuk1`
**WHY:** 🔴 See bucket permissions

```json
{
    "Policy": "{\n  \"Version\": \"2012-10-17\",\n  \"Statement\": [\n    {\n      \"Effect\": \"Allow\",\n      \"Principal\": \"*\",\n      \"Action\": \"s3:GetObject\",\n      \"Resource\": \"arn:aws:s3:::securecopbakupbuk1/*\"\n    }\n  ]\n}"
}
```

### 🔵 `aws s3api delete-bucket-policy --bucket securecopbakupbuk1`
**WHY:** Remove bucket policy

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 BUCKET ACL COMMANDS

### 🔵 `aws s3api get-bucket-acl --bucket securecopbakupbuk1`
**WHY:** See bucket access control list

```json
{
    "Owner": {
        "ID": "bcaf1ffd86f41161ca5fb16fd081034f"
    },
    "Grants": [
        {
            "Grantee": {
                "ID": "bcaf1ffd86f41161ca5fb16fd081034f",
                "Type": "CanonicalUser"
            },
            🔴 "Permission": "FULL_CONTROL"
        }
    ]
}
```

### 🔵 `aws s3api put-bucket-acl --bucket securecopbakupbuk1 --acl private`
**WHY:** Set bucket ACL (private, public-read, etc.)

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws s3api put-bucket-acl --bucket securecopbakupbuk1 --grant-read uri=http://acs.amazonaws.com/groups/global/AllUsers`
**WHY:** Grant public read access

---

## 🔵 BUCKET VERSIONING COMMANDS

### 🔵 `aws s3api get-bucket-versioning --bucket securecopbakupbuk1`
**WHY:** Check if versioning is enabled

```json
{
    🔴 "Status": "Enabled"
}
```

### 🔵 `aws s3api put-bucket-versioning --bucket securecopbakupbuk1 --versioning-configuration Status=Enabled`
**WHY:** Enable versioning (keeps old versions)

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws s3api list-object-versions --bucket securecopbakupbuk1`
**WHY:** 🔴 See all versions of files (including deleted!)

```json
{
    "Versions": [
        {
            🔴 "Key": "key.txt",
            🔴 "VersionId": "12345",
            "IsLatest": true
        }
    ],
    "DeleteMarkers": [
        {
            🔴 "Key": "oldfile.txt",
            🔴 "VersionId": "67890"
        }
    ]
}
```

---

## 🔵 BUCKET ENCRYPTION COMMANDS

### 🔵 `aws s3api get-bucket-encryption --bucket securecopbakupbuk1`
**WHY:** Check bucket encryption settings

```json
{
    "ServerSideEncryptionConfiguration": {
        "Rules": [
            {
                "ApplyServerSideEncryptionByDefault": {
                    🔴 "SSEAlgorithm": "AES256"
                }
            }
        ]
    }
}
```

### 🔵 `aws s3api put-bucket-encryption --bucket securecopbakupbuk1 --server-side-encryption-configuration file://encryption.json`
**WHY:** Set bucket encryption

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws s3api delete-bucket-encryption --bucket securecopbakupbuk1`
**WHY:** Remove bucket encryption

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 BUCKET CORS COMMANDS

### 🔵 `aws s3api get-bucket-cors --bucket securecopbakupbuk1`
**WHY:** Check CORS configuration

```json
{
    "CORSRules": [
        {
            "AllowedOrigins": ["https://example.com"],
            "AllowedMethods": ["GET", "PUT"],
            "AllowedHeaders": ["*"]
        }
    ]
}
```

### 🔵 `aws s3api put-bucket-cors --bucket securecopbakupbuk1 --cors-configuration file://cors.json`
**WHY:** Set CORS rules

### 🔵 `aws s3api delete-bucket-cors --bucket securecopbakupbuk1`
**WHY:** Remove CORS

---

## 🔵 BUCKET LOCATION COMMANDS

### 🔵 `aws s3api get-bucket-location --bucket securecopbakupbuk1`
**WHY:** Find bucket region

```json
{
    🔴 "LocationConstraint": "us-east-1"
}
```

---

## 🔵 OBJECT TAGGING COMMANDS

### 🔵 `aws s3api get-object-tagging --bucket securecopbakupbuk1 --key key.txt`
**WHY:** See tags on object

```json
{
    "TagSet": [
        {
            🔴 "Key": "Confidential",
            🔴 "Value": "True"
        }
    ]
}
```

### 🔵 `aws s3api put-object-tagging --bucket securecopbakupbuk1 --key key.txt --tagging file://tags.json`
**WHY:** Add tags to object

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws s3api delete-object-tagging --bucket securecopbakupbuk1 --key key.txt`
**WHY:** Remove tags

---

## 🔵 MULTIPART UPLOAD COMMANDS

### 🔵 `aws s3api create-multipart-upload --bucket securecopbakupbuk1 --key largefile.zip`
**WHY:** Start multipart upload for large files

```json
{
    🔴 "UploadId": "12345"
}
```

### 🔵 `aws s3api upload-part --bucket securecopbakupbuk1 --key largefile.zip --part-number 1 --body part1 --upload-id 12345`
**WHY:** Upload a part

### 🔵 `aws s3api complete-multipart-upload --bucket securecopbakupbuk1 --key largefile.zip --upload-id 12345 --multipart-upload file://parts.json`
**WHY:** Complete multipart upload

### 🔵 `aws s3api abort-multipart-upload --bucket securecopbakupbuk1 --key largefile.zip --upload-id 12345`
**WHY:** Cancel multipart upload

---

# PART 6: EC2 (ELASTIC COMPUTE CLOUD) - ALL COMMANDS!

## 🔵 INSTANCE COMMANDS

### 🔵 `aws ec2 describe-instances`
**WHY:** 🔴 See all EC2 instances

```json
{
    "Reservations": [
        {
            "Instances": [
                {
                    🔴 "InstanceId": "i-1234567890abcdef0",
                    🔴 "InstanceType": "t2.micro",
                    🔴 "State": {"Name": "running"},
                    🔴 "PublicIpAddress": "54.123.45.67",
                    🔴 "PrivateIpAddress": "172.31.45.123"
                }
            ]
        }
    ]
}
```

### 🔵 `aws ec2 describe-instances --instance-ids i-1234567890abcdef0`
**WHY:** Get specific instance details

### 🔵 `aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"`
**WHY:** Filter instances by state

### 🔵 `aws ec2 describe-instances --query "Reservations[].Instances[].InstanceId" --output text`
**WHY:** Get only instance IDs (for scripting)

```
i-1234567890abcdef0
i-0987654321fedcba0
```

### 🔵 `aws ec2 run-instances --image-id ami-0c55b159cbfafe1f0 --instance-type t2.micro --key-name my-key-pair --security-group-ids sg-12345678 --subnet-id subnet-12345678 --count 1`
**WHY:** 🔴 Launch new EC2 instance

```json
{
    "Instances": [
        {
            🔴 "InstanceId": "i-1234567890abcdef1",
            "InstanceType": "t2.micro",
            🔴 "State": {"Name": "pending"}
        }
    ]
}
```

### 🔵 `aws ec2 start-instances --instance-ids i-1234567890abcdef1`
**WHY:** Start stopped instance

```json
{
    "StartingInstances": [
        {
            "InstanceId": "i-1234567890abcdef1",
            🔴 "CurrentState": {"Name": "starting"},
            🔴 "PreviousState": {"Name": "stopped"}
        }
    ]
}
```

### 🔵 `aws ec2 stop-instances --instance-ids i-1234567890abcdef1`
**WHY:** Stop running instance

```json
{
    "StoppingInstances": [
        {
            "InstanceId": "i-1234567890abcdef1",
            🔴 "CurrentState": {"Name": "stopping"},
            🔴 "PreviousState": {"Name": "running"}
        }
    ]
}
```

### 🔵 `aws ec2 reboot-instances --instance-ids i-1234567890abcdef1`
**WHY:** Reboot instance

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws ec2 terminate-instances --instance-ids i-1234567890abcdef1`
**WHY:** 🔴 **PERMANENTLY DELETE INSTANCE!** (Can't undo)

```json
{
    "TerminatingInstances": [
        {
            "InstanceId": "i-1234567890abcdef1",
            🔴 "CurrentState": {"Name": "shutting-down"},
            🔴 "PreviousState": {"Name": "running"}
        }
    ]
}
```

---

## 🔵 KEY PAIR COMMANDS

### 🔵 `aws ec2 describe-key-pairs`
**WHY:** List all SSH key pairs

```json
{
    "KeyPairs": [
        {
            🔴 "KeyName": "my-key-pair",
            "KeyFingerprint": "12:34:56:78:90:ab:cd:ef"
        }
    ]
}
```

### 🔵 `aws ec2 create-key-pair --key-name my-new-key --query 'KeyMaterial' --output text > my-new-key.pem`
**WHY:** 🔴 Create SSH key and save private key

```
🟢 (Private key saved to my-new-key.pem)
```

### 🔵 `aws ec2 delete-key-pair --key-name my-key-pair`
**WHY:** Delete key pair

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws ec2 import-key-pair --key-name imported-key --public-key-material fileb://public-key.pub`
**WHY:** Import existing public key

```json
{
    🔴 "KeyName": "imported-key",
    "KeyFingerprint": "ab:cd:ef:12:34:56:78:90"
}
```

---

## 🔵 SECURITY GROUP COMMANDS (FIREWALL)

### 🔵 `aws ec2 describe-security-groups`
**WHY:** List all security groups

```json
{
    "SecurityGroups": [
        {
            🔴 "GroupId": "sg-1234567890abcdef0",
            🔴 "GroupName": "my-security-group",
            "Description": "My security group"
        }
    ]
}
```

### 🔵 `aws ec2 create-security-group --group-name my-security-group --description "My firewall rules" --vpc-id vpc-12345678`
**WHY:** Create new security group

```json
{
    🔴 "GroupId": "sg-1234567890abcdef0"
}
```

### 🔵 `aws ec2 delete-security-group --group-id sg-1234567890abcdef0`
**WHY:** Delete security group

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws ec2 authorize-security-group-ingress --group-id sg-1234567890abcdef0 --protocol tcp --port 22 --cidr 0.0.0.0/0`
**WHY:** 🔴 Allow SSH from ANYWHERE (dangerous!)

```json
{
    "Return": true,
    "SecurityGroupRules": [
        {
            🔴 "CidrIpv4": "0.0.0.0/0",
            🔴 "FromPort": 22,
            🔴 "ToPort": 22,
            🔴 "IpProtocol": "tcp"
        }
    ]
}
```

### 🔵 `aws ec2 authorize-security-group-ingress --group-id sg-1234567890abcdef0 --protocol tcp --port 80 --cidr 0.0.0.0/0`
**WHY:** Allow HTTP from anywhere

### 🔵 `aws ec2 authorize-security-group-ingress --group-id sg-1234567890abcdef0 --protocol tcp --port 443 --cidr 0.0.0.0/0`
**WHY:** Allow HTTPS from anywhere

### 🔵 `aws ec2 authorize-security-group-ingress --group-id sg-1234567890abcdef0 --protocol tcp --port 3389 --cidr 192.168.1.0/24`
**WHY:** Allow RDP only from specific IP range

### 🔵 `aws ec2 revoke-security-group-ingress --group-id sg-1234567890abcdef0 --protocol tcp --port 22 --cidr 0.0.0.0/0`
**WHY:** Remove SSH access rule

```json
{
    "Return": true
}
```

### 🔵 `aws ec2 authorize-security-group-egress --group-id sg-1234567890abcdef0 --protocol tcp --port 443 --cidr 0.0.0.0/0`
**WHY:** Allow outbound HTTPS

### 🔵 `aws ec2 revoke-security-group-egress --group-id sg-1234567890abcdef0 --protocol tcp --port 443 --cidr 0.0.0.0/0`
**WHY:** Remove outbound rule

---

## 🔵 ELASTIC IP COMMANDS

### 🔵 `aws ec2 allocate-address --domain vpc`
**WHY:** Get a new public IP address

```json
{
    🔴 "PublicIp": "54.123.45.68",
    🔴 "AllocationId": "eipalloc-12345678",
    "Domain": "vpc"
}
```

### 🔵 `aws ec2 describe-addresses`
**WHY:** List all Elastic IPs

```json
{
    "Addresses": [
        {
            🔴 "PublicIp": "54.123.45.68",
            🔴 "AllocationId": "eipalloc-12345678"
        }
    ]
}
```

### 🔵 `aws ec2 associate-address --instance-id i-1234567890abcdef1 --allocation-id eipalloc-12345678`
**WHY:** Attach public IP to instance

```json
{
    🔴 "AssociationId": "eipassoc-12345678"
}
```

### 🔵 `aws ec2 disassociate-address --association-id eipassoc-12345678`
**WHY:** Detach public IP from instance

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws ec2 release-address --allocation-id eipalloc-12345678`
**WHY:** 🔴 Release public IP (give it back)

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 EBS VOLUME COMMANDS (DISKS)

### 🔵 `aws ec2 describe-volumes`
**WHY:** List all EBS volumes

```json
{
    "Volumes": [
        {
            🔴 "VolumeId": "vol-1234567890abcdef0",
            🔴 "Size": 10,
            🔴 "State": "available",
            🔴 "AvailabilityZone": "us-east-1a"
        }
    ]
}
```

### 🔵 `aws ec2 create-volume --availability-zone us-east-1a --size 10 --volume-type gp2`
**WHY:** Create new 10GB disk

```json
{
    🔴 "VolumeId": "vol-1234567890abcdef0",
    🔴 "Size": 10,
    "State": "creating"
}
```

### 🔵 `aws ec2 delete-volume --volume-id vol-1234567890abcdef0`
**WHY:** 🔴 Delete volume

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws ec2 attach-volume --volume-id vol-1234567890abcdef0 --instance-id i-1234567890abcdef1 --device /dev/sdf`
**WHY:** Attach disk to instance

```json
{
    "VolumeId": "vol-1234567890abcdef0",
    "InstanceId": "i-1234567890abcdef1",
    "Device": "/dev/sdf",
    🔴 "State": "attaching"
}
```

### 🔵 `aws ec2 detach-volume --volume-id vol-1234567890abcdef0`
**WHY:** Detach disk from instance

```json
{
    "VolumeId": "vol-1234567890abcdef0",
    "InstanceId": "i-1234567890abcdef1",
    🔴 "State": "detaching"
}
```

### 🔵 `aws ec2 modify-volume --volume-id vol-1234567890abcdef0 --size 20`
**WHY:** Increase disk size

```json
{
    "VolumeModification": {
        🔴 "VolumeId": "vol-1234567890abcdef0",
        🔴 "TargetSize": 20,
        🔴 "ModificationState": "modifying"
    }
}
```

---

## 🔵 SNAPSHOT COMMANDS (BACKUPS)

### 🔵 `aws ec2 create-snapshot --volume-id vol-1234567890abcdef0 --description "Backup before update"`
**WHY:** 🔴 **BACKUP YOUR DISK!** (Always do before changes)

```json
{
    🔴 "SnapshotId": "snap-1234567890abcdef0",
    "VolumeId": "vol-1234567890abcdef0",
    🔴 "State": "pending",
    🔴 "Description": "Backup before update"
}
```

### 🔵 `aws ec2 describe-snapshots`
**WHY:** List all snapshots

```json
{
    "Snapshots": [
        {
            🔴 "SnapshotId": "snap-1234567890abcdef0",
            "VolumeId": "vol-1234567890abcdef0",
            🔴 "State": "completed"
        }
    ]
}
```

### 🔵 `aws ec2 delete-snapshot --snapshot-id snap-1234567890abcdef0`
**WHY:** Delete snapshot

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws ec2 copy-snapshot --source-region us-east-1 --source-snapshot-id snap-1234567890abcdef0 --region us-west-2 --description "Copy of backup"`
**WHY:** Copy snapshot to another region

```json
{
    🔴 "SnapshotId": "snap-0987654321fedcba0"
}
```

---

## 🔵 AMI COMMANDS (MACHINE IMAGES)

### 🔵 `aws ec2 describe-images --owners amazon`
**WHY:** List Amazon-provided AMIs

### 🔵 `aws ec2 describe-images --filters "Name=name,Values=amzn2-ami-hvm-2.0.*"`
**WHY:** Find specific AMI

```json
{
    "Images": [
        {
            🔴 "ImageId": "ami-0c55b159cbfafe1f0",
            🔴 "Name": "amzn2-ami-hvm-2.0.20250301-x86_64-gp2"
        }
    ]
}
```

### 🔵 `aws ec2 create-image --instance-id i-1234567890abcdef1 --name "My Server Image" --description "Backup of my server"`
**WHY:** Create AMI from instance

```json
{
    🔴 "ImageId": "ami-1234567890abcdef0"
}
```

### 🔵 `aws ec2 deregister-image --image-id ami-1234567890abcdef0`
**WHY:** Delete AMI

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 TAGS COMMANDS

### 🔵 `aws ec2 create-tags --resources i-1234567890abcdef1 --tags Key=Name,Value=MyWebServer Key=Environment,Value=Production`
**WHY:** Add tags to resource

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws ec2 describe-tags --filters "Name=resource-id,Values=i-1234567890abcdef1"`
**WHY:** See tags on resource

```json
{
    "Tags": [
        {
            🔴 "Key": "Name",
            🔴 "Value": "MyWebServer"
        },
        {
            🔴 "Key": "Environment",
            🔴 "Value": "Production"
        }
    ]
}
```

### 🔵 `aws ec2 delete-tags --resources i-1234567890abcdef1 --tags Key=Name`
**WHY:** Remove specific tag

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 NETWORK COMMANDS

### 🔵 `aws ec2 describe-vpcs`
**WHY:** List all VPCs

```json
{
    "Vpcs": [
        {
            🔴 "VpcId": "vpc-12345678",
            🔴 "CidrBlock": "10.0.0.0/16",
            "IsDefault": true
        }
    ]
}
```

### 🔵 `aws ec2 describe-subnets`
**WHY:** List all subnets

```json
{
    "Subnets": [
        {
            🔴 "SubnetId": "subnet-12345678",
            🔴 "VpcId": "vpc-12345678",
            🔴 "CidrBlock": "10.0.1.0/24",
            🔴 "AvailabilityZone": "us-east-1a"
        }
    ]
}
```

### 🔵 `aws ec2 describe-internet-gateways`
**WHY:** List internet gateways

### 🔵 `aws ec2 describe-route-tables`
**WHY:** List route tables

### 🔵 `aws ec2 describe-network-interfaces`
**WHY:** List network interfaces (ENI)

```json
{
    "NetworkInterfaces": [
        {
            🔴 "NetworkInterfaceId": "eni-12345678",
            🔴 "PrivateIpAddress": "10.0.1.123"
        }
    ]
}
```

---

# PART 7: KMS (KEY MANAGEMENT SERVICE) - ALL COMMANDS!

## 🔵 KEY COMMANDS

### 🔵 `aws kms list-keys`
**WHY:** 🔴 See all encryption keys

```json
{
    "Keys": [
        {
            🔴 "KeyId": "a7a251b3-c889-4dd1-9176-931c207c33d5",
            🔴 "KeyArn": "arn:aws:kms:us-east-1:058264439561:key/a7a251b3-c889-4dd1-9176-931c207c33d5"
        }
    ]
}
```

### 🔵 `aws kms describe-key --key-id a7a251b3-c889-4dd1-9176-931c207c33d5`
**WHY:** 🔴 Get key details

```json
{
    "KeyMetadata": {
        🔴 "KeyId": "a7a251b3-c889-4dd1-9176-931c207c33d5",
        🔴 "Arn": "arn:aws:kms:us-east-1:058264439561:key/a7a251b3-c889-4dd1-9176-931c207c33d5",
        🔴 "Enabled": true,
        🔴 "KeyState": "Enabled",
        🔴 "Description": "Key for S3 encryption"
    }
}
```

### 🔵 `aws kms create-key --description "My new KMS key"`
**WHY:** Create new KMS key

```json
{
    "KeyMetadata": {
        🔴 "KeyId": "b8b362c4-d990-5ee2-8277-8a9c2a1b3d4e",
        🔴 "Arn": "arn:aws:kms:us-east-1:058264439561:key/b8b362c4-d990-5ee2-8277-8a9c2a1b3d4e",
        "Enabled": true
    }
}
```

### 🔵 `aws kms schedule-key-deletion --key-id a7a251b3-c889-4dd1-9176-931c207c33d5 --pending-window-in-days 7`
**WHY:** 🔴 Schedule key deletion (DANGEROUS!)

```json
{
    "KeyId": "a7a251b3-c889-4dd1-9176-931c207c33d5",
    🔴 "DeletionDate": "2025-03-17T15:45:22+00:00"
}
```

### 🔵 `aws kms cancel-key-deletion --key-id a7a251b3-c889-4dd1-9176-931c207c33d5`
**WHY:** Stop key deletion

```json
{
    🔴 "KeyId": "a7a251b3-c889-4dd1-9176-931c207c33d5"
}
```

### 🔵 `aws kms enable-key --key-id a7a251b3-c889-4dd1-9176-931c207c33d5`
**WHY:** Enable key

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws kms disable-key --key-id a7a251b3-c889-4dd1-9176-931c207c33d5`
**WHY:** 🔴 Disable key (breaks everything using it!)

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 ALIAS COMMANDS

### 🔵 `aws kms list-aliases`
**WHY:** List all key aliases

```json
{
    "Aliases": [
        {
            🔴 "AliasName": "alias/s3-key",
            🔴 "AliasArn": "arn:aws:kms:us-east-1:058264439561:alias/s3-key",
            🔴 "TargetKeyId": "a7a251b3-c889-4dd1-9176-931c207c33d5"
        }
    ]
}
```

### 🔵 `aws kms create-alias --alias-name alias/my-new-key --target-key-id b8b362c4-d990-5ee2-8277-8a9c2a1b3d4e`
**WHY:** Create alias for key

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws kms delete-alias --alias-name alias/my-new-key`
**WHY:** Delete alias

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 KEY POLICY COMMANDS

### 🔵 `aws kms get-key-policy --key-id a7a251b3-c889-4dd1-9176-931c207c33d5 --policy-name default`
**WHY:** 🔴 See who can use the key

```json
{
    "Policy": "{\n  \"Version\": \"2012-10-17\",\n  \"Statement\": [\n    {\n      \"Effect\": \"Allow\",\n      \"Principal\": {\n        \"AWS\": \"arn:aws:iam::058264439561:user/BackupReader1\"\n      },\n      \"Action\": \"kms:Decrypt\",\n      \"Resource\": \"*\"\n    }\n  ]\n}"
}
```

### 🔵 `aws kms put-key-policy --key-id a7a251b3-c889-4dd1-9176-931c207c33d5 --policy-name default --policy file://new-policy.json`
**WHY:** Change key permissions

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 ENCRYPTION/DECRYPTION COMMANDS

### 🔵 `aws kms encrypt --key-id a7a251b3-c889-4dd1-9176-931c207c33d5 --plaintext fileb://secret.txt --output text --query CiphertextBlob > encrypted.base64`
**WHY:** Encrypt data with KMS

### 🔵 `aws kms decrypt --ciphertext-blob fileb://encrypted-file --output text --query Plaintext | base64 --decode > decrypted.txt`
**WHY:** 🔴 Decrypt data with KMS

---

# PART 8: LAMBDA COMMANDS (SERVERLESS)

## 🔵 FUNCTION COMMANDS

### 🔵 `aws lambda list-functions`
**WHY:** List all Lambda functions

```json
{
    "Functions": [
        {
            🔴 "FunctionName": "my-function",
            🔴 "Runtime": "nodejs18.x",
            🔴 "Handler": "index.handler",
            🔴 "LastModified": "2025-02-20T14:30:22+00:00"
        }
    ]
}
```

### 🔵 `aws lambda get-function --function-name my-function`
**WHY:** Get function details

```json
{
    "Configuration": {
        🔴 "FunctionName": "my-function",
        🔴 "Runtime": "nodejs18.x",
        🔴 "Role": "arn:aws:iam::058264439561:role/lambda-role"
    }
}
```

### 🔵 `aws lambda create-function --function-name my-new-function --runtime nodejs18.x --role arn:aws:iam::058264439561:role/lambda-role --handler index.handler --zip-file fileb://function.zip`
**WHY:** Create new Lambda function

```json
{
    🔴 "FunctionName": "my-new-function",
    "FunctionArn": "arn:aws:lambda:us-east-1:058264439561:function:my-new-function"
}
```

### 🔵 `aws lambda delete-function --function-name my-new-function`
**WHY:** Delete function

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws lambda update-function-code --function-name my-function --zip-file fileb://new-code.zip`
**WHY:** Update function code

```json
{
    🔴 "LastModified": "2025-03-10T16:30:22+00:00"
}
```

### 🔵 `aws lambda invoke --function-name my-function output.txt`
**WHY:** 🔴 Run the function

```json
{
    🔴 "StatusCode": 200
}
```

### 🔵 `aws lambda invoke --function-name my-function --payload file://event.json output.txt`
**WHY:** Run with input data

---

## 🔵 PERMISSION COMMANDS

### 🔵 `aws lambda add-permission --function-name my-function --statement-id AllowS3Invoke --action lambda:InvokeFunction --principal s3.amazonaws.com --source-arn arn:aws:s3:::my-bucket`
**WHY:** Allow S3 to trigger Lambda

### 🔵 `aws lambda remove-permission --function-name my-function --statement-id AllowS3Invoke`
**WHY:** Remove permission

---

# PART 9: DYNAMODB COMMANDS (NoSQL DATABASE)

## 🔵 TABLE COMMANDS

### 🔵 `aws dynamodb list-tables`
**WHY:** List all DynamoDB tables

```json
{
    🔴 "TableNames": ["Users", "Orders"]
}
```

### 🔵 `aws dynamodb describe-table --table-name Users`
**WHY:** Get table details

```json
{
    "Table": {
        🔴 "TableName": "Users",
        🔴 "TableStatus": "ACTIVE",
        "KeySchema": [
            {
                🔴 "AttributeName": "UserId",
                "KeyType": "HASH"
            }
        ]
    }
}
```

### 🔵 `aws dynamodb create-table --table-name Users --attribute-definitions AttributeName=UserId,AttributeType=S --key-schema AttributeName=UserId,KeyType=HASH --billing-mode PAY_PER_REQUEST`
**WHY:** Create new table

```json
{
    "TableDescription": {
        🔴 "TableName": "Users",
        🔴 "TableStatus": "CREATING"
    }
}
```

### 🔵 `aws dynamodb delete-table --table-name Users`
**WHY:** 🔴 Delete table

```json
{
    "TableDescription": {
        🔴 "TableStatus": "DELETING"
    }
}
```

---

## 🔵 ITEM COMMANDS

### 🔵 `aws dynamodb scan --table-name Users`
**WHY:** 🔴 Get ALL items from table (expensive!)

```json
{
    "Items": [
        {
            "UserId": {🔴 "S": "user123"},
            "Name": {🔴 "S": "John Doe"},
            "Email": {🔴 "S": "john@example.com"}
        }
    ],
    🔴 "Count": 1
}
```

### 🔵 `aws dynamodb get-item --table-name Users --key '{"UserId":{"S":"user123"}}'`
**WHY:** Get single item (cheaper)

```json
{
    "Item": {
        "UserId": {"S": "user123"},
        "Name": {"S": "John Doe"},
        "Email": {"S": "john@example.com"}
    }
}
```

### 🔵 `aws dynamodb put-item --table-name Users --item '{"UserId":{"S":"user456"},"Name":{"S":"Jane Smith"},"Email":{"S":"jane@example.com"}}'`
**WHY:** Insert or replace item

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws dynamodb update-item --table-name Users --key '{"UserId":{"S":"user456"}}' --update-expression "SET #n = :newName" --expression-attribute-names '{"#n":"Name"}' --expression-attribute-values '{":newName":{"S":"Jane Doe"}}'`
**WHY:** Update specific attributes

### 🔵 `aws dynamodb delete-item --table-name Users --key '{"UserId":{"S":"user456"}}'`
**WHY:** Delete item

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws dynamodb query --table-name Users --key-condition-expression "UserId = :id" --expression-attribute-values '{":id":{"S":"user123"}}'`
**WHY:** Query by primary key (more efficient than scan)

---

# PART 10: SQS (SIMPLE QUEUE SERVICE) - ALL COMMANDS!

## 🔵 QUEUE COMMANDS

### 🔵 `aws sqs list-queues`
**WHY:** List all queues

```json
{
    "QueueUrls": [
        🔴 "https://sqs.us-east-1.amazonaws.com/058264439561/my-queue"
    ]
}
```

### 🔵 `aws sqs create-queue --queue-name my-new-queue`
**WHY:** Create new queue

```json
{
    🔴 "QueueUrl": "https://sqs.us-east-1.amazonaws.com/058264439561/my-new-queue"
}
```

### 🔵 `aws sqs get-queue-attributes --queue-url https://sqs.us-east-1.amazonaws.com/058264439561/my-queue --attribute-names All`
**WHY:** Get queue properties

```json
{
    "Attributes": {
        🔴 "QueueArn": "arn:aws:sqs:us-east-1:058264439561:my-queue",
        🔴 "ApproximateNumberOfMessages": "0",
        🔴 "VisibilityTimeout": "30"
    }
}
```

### 🔵 `aws sqs delete-queue --queue-url https://sqs.us-east-1.amazonaws.com/058264439561/my-new-queue`
**WHY:** Delete queue

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 MESSAGE COMMANDS

### 🔵 `aws sqs send-message --queue-url https://sqs.us-east-1.amazonaws.com/058264439561/my-queue --message-body "Hello world"`
**WHY:** Send message to queue

```json
{
    🔴 "MessageId": "12345678-1234-1234-1234-123456789012",
    "MD5OfMessageBody": "5d41402abc4b2a76b9719d911017c592"
}
```

### 🔵 `aws sqs send-message-batch --queue-url https://sqs.us-east-1.amazonaws.com/058264439561/my-queue --entries file://messages.json`
**WHY:** Send multiple messages

### 🔵 `aws sqs receive-message --queue-url https://sqs.us-east-1.amazonaws.com/058264439561/my-queue`
**WHY:** 🔴 Get messages from queue

```json
{
    "Messages": [
        {
            🔴 "MessageId": "12345678-1234-1234-1234-123456789012",
            🔴 "ReceiptHandle": "AQEBwJnK...",
            🔴 "Body": "Hello world"
        }
    ]
}
```

### 🔵 `aws sqs receive-message --queue-url https://sqs.us-east-1.amazonaws.com/058264439561/my-queue --max-number-of-messages 10 --visibility-timeout 60`
**WHY:** Get up to 10 messages

### 🔵 `aws sqs delete-message --queue-url https://sqs.us-east-1.amazonaws.com/058264439561/my-queue --receipt-handle "AQEBwJnK..."`
**WHY:** Remove message after processing

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws sqs purge-queue --queue-url https://sqs.us-east-1.amazonaws.com/058264439561/my-queue`
**WHY:** 🔴 Delete ALL messages in queue

```
🟢 (No output = SUCCESS!)
```

---

# PART 11: SNS (SIMPLE NOTIFICATION SERVICE)

## 🔵 TOPIC COMMANDS

### 🔵 `aws sns list-topics`
**WHY:** List all SNS topics

```json
{
    "Topics": [
        {
            🔴 "TopicArn": "arn:aws:sns:us-east-1:058264439561:my-topic"
        }
    ]
}
```

### 🔵 `aws sns create-topic --name my-new-topic`
**WHY:** Create new topic

```json
{
    🔴 "TopicArn": "arn:aws:sns:us-east-1:058264439561:my-new-topic"
}
```

### 🔵 `aws sns delete-topic --topic-arn arn:aws:sns:us-east-1:058264439561:my-new-topic`
**WHY:** Delete topic

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 SUBSCRIPTION COMMANDS

### 🔵 `aws sns list-subscriptions`
**WHY:** List all subscriptions

### 🔵 `aws sns subscribe --topic-arn arn:aws:sns:us-east-1:058264439561:my-topic --protocol email --notification-endpoint user@example.com`
**WHY:** Subscribe email to topic

```json
{
    🔴 "SubscriptionArn": "pending"
}
```

### 🔵 `aws sns subscribe --topic-arn arn:aws:sns:us-east-1:058264439561:my-topic --protocol sqs --notification-endpoint arn:aws:sqs:us-east-1:058264439561:my-queue`
**WHY:** Subscribe SQS queue to topic

### 🔵 `aws sns unsubscribe --subscription-arn arn:aws:sns:us-east-1:058264439561:my-topic:12345678-1234-1234-1234-123456789012`
**WHY:** Remove subscription

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 PUBLISH COMMANDS

### 🔵 `aws sns publish --topic-arn arn:aws:sns:us-east-1:058264439561:my-topic --message "Hello subscribers" --subject "Test Message"`
**WHY:** Send message to topic

```json
{
    🔴 "MessageId": "12345678-1234-1234-1234-123456789012"
}
```

### 🔵 `aws sns publish --phone-number "+1234567890" --message "SMS test"`
**WHY:** Send SMS message

---

# PART 12: CLOUDWATCH LOGS COMMANDS

## 🔵 LOG GROUP COMMANDS

### 🔵 `aws logs describe-log-groups`
**WHY:** List all log groups

```json
{
    "logGroups": [
        {
            🔴 "logGroupName": "/aws/lambda/my-function",
            🔴 "creationTime": 1234567890123,
            🔴 "storedBytes": 12345
        }
    ]
}
```

### 🔵 `aws logs create-log-group --log-group-name /aws/lambda/new-function`
**WHY:** Create log group

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws logs delete-log-group --log-group-name /aws/lambda/new-function`
**WHY:** Delete log group

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 LOG STREAM COMMANDS

### 🔵 `aws logs describe-log-streams --log-group-name /aws/lambda/my-function`
**WHY:** List log streams in group

```json
{
    "logStreams": [
        {
            🔴 "logStreamName": "2025/03/10/[$LATEST]123456",
            🔴 "creationTime": 1234567890123
        }
    ]
}
```

### 🔵 `aws logs create-log-stream --log-group-name /aws/lambda/my-function --log-stream-name new-stream`
**WHY:** Create log stream

---

## 🔵 LOG EVENT COMMANDS

### 🔵 `aws logs get-log-events --log-group-name /aws/lambda/my-function --log-stream-name "2025/03/10/[$LATEST]123456"`
**WHY:** 🔴 Read log entries

```json
{
    "events": [
        {
            "timestamp": 1234567890123,
            🔴 "message": "Function started"
        },
        {
            "timestamp": 1234567890456,
            🔴 "message": "Error: Something broke!"
        }
    ]
}
```

### 🔵 `aws logs put-log-events --log-group-name /aws/lambda/my-function --log-stream-name new-stream --log-events file://logs.json`
**WHY:** Write log entries

---

## 🔵 METRIC FILTER COMMANDS

### 🔵 `aws logs describe-metric-filters --log-group-name /aws/lambda/my-function`
**WHY:** List metric filters

---

# PART 13: CLOUDWATCH METRICS COMMANDS

## 🔵 METRIC COMMANDS

### 🔵 `aws cloudwatch list-metrics`
**WHY:** List all available metrics

```json
{
    "Metrics": [
        {
            🔴 "Namespace": "AWS/EC2",
            🔴 "MetricName": "CPUUtilization"
        }
    ]
}
```

### 🔵 `aws cloudwatch list-metrics --namespace AWS/EC2`
**WHY:** List EC2 metrics only

### 🔵 `aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization --dimensions Name=InstanceId,Value=i-1234567890abcdef0 --start-time 2025-03-09T00:00:00Z --end-time 2025-03-10T00:00:00Z --period 3600 --statistics Average`
**WHY:** Get metric data

```json
{
    "Datapoints": [
        {
            "Timestamp": "2025-03-09T10:00:00Z",
            🔴 "Average": 12.5,
            "Unit": "Percent"
        }
    ]
}
```

---

## 🔵 ALARM COMMANDS

### 🔵 `aws cloudwatch describe-alarms`
**WHY:** List all alarms

```json
{
    "MetricAlarms": [
        {
            🔴 "AlarmName": "High CPU Alarm",
            🔴 "StateValue": "OK"
        }
    ]
}
```

### 🔵 `aws cloudwatch put-metric-alarm --alarm-name HighCPU --alarm-description "Alert when CPU > 80%" --metric-name CPUUtilization --namespace AWS/EC2 --statistic Average --period 300 --threshold 80 --comparison-operator GreaterThanThreshold --evaluation-periods 2 --actions-enabled --alarm-actions arn:aws:sns:us-east-1:058264439561:my-topic`
**WHY:** Create alarm

```json
🟢 (No output = SUCCESS!)
```

### 🔵 `aws cloudwatch delete-alarms --alarm-names HighCPU`
**WHY:** Delete alarm

```
🟢 (No output = SUCCESS!)
```

### 🔵 `aws cloudwatch set-alarm-state --alarm-name HighCPU --state-value ALARM --state-reason "Testing"`
**WHY:** Manually trigger alarm for testing

---

# PART 14: SECRETS MANAGER COMMANDS

## 🔵 SECRET COMMANDS

### 🔵 `aws secretsmanager list-secrets`
**WHY:** 🔴 List all stored secrets

```json
{
    "SecretList": [
        {
            🔴 "Name": "database-password",
            🔴 "ARN": "arn:aws:secretsmanager:us-east-1:058264439561:secret:database-password-123abc"
        }
    ]
}
```

### 🔵 `aws secretsmanager get-secret-value --secret-id database-password`
**WHY:** 🔴 **GET THE ACTUAL SECRET!** (EXPLOIT!)

```json
{
    "Name": "database-password",
    🔴 "SecretString": "{\"username\":\"admin\",\"password\":\"SuperSecret123!\"}",
    🔴 "VersionId": "12345678-1234-1234-1234-123456789012"
}
```

### 🔵 `aws secretsmanager get-secret-value --secret-id database-password --version-stage AWSCURRENT`
**WHY:** Get current version

### 🔵 `aws secretsmanager create-secret --name my-new-secret --secret-string "{\"apiKey\":\"abc123\"}"`
**WHY:** Create new secret

```json
{
    🔴 "ARN": "arn:aws:secretsmanager:us-east-1:058264439561:secret:my-new-secret-456def",
    🔴 "Name": "my-new-secret"
}
```

### 🔵 `aws secretsmanager put-secret-value --secret-id my-new-secret --secret-string "{\"apiKey\":\"newkey456\"}"`
**WHY:** Update secret value

### 🔵 `aws secretsmanager delete-secret --secret-id my-new-secret`
**WHY:** 🔴 Delete secret

```json
{
    "ARN": "arn:aws:secretsmanager:us-east-1:058264439561:secret:my-new-secret-456def",
    "Name": "my-new-secret",
    🔴 "DeletionDate": "2025-04-09T12:00:00+00:00"
}
```

### 🔵 `aws secretsmanager restore-secret --secret-id my-new-secret`
**WHY:** Restore scheduled deletion

---

# PART 15: CLOUDFORMATION COMMANDS (Infrastructure as Code)

## 🔵 STACK COMMANDS

### 🔵 `aws cloudformation list-stacks`
**WHY:** List all CloudFormation stacks

```json
{
    "StackSummaries": [
        {
            🔴 "StackName": "my-stack",
            🔴 "StackStatus": "CREATE_COMPLETE"
        }
    ]
}
```

### 🔵 `aws cloudformation describe-stacks --stack-name my-stack`
**WHY:** Get stack details

```json
{
    "Stacks": [
        {
            🔴 "StackName": "my-stack",
            🔴 "StackStatus": "CREATE_COMPLETE",
            "Parameters": [...],
            "Outputs": [...]
        }
    ]
}
```

### 🔵 `aws cloudformation create-stack --stack-name my-new-stack --template-body file://template.yaml`
**WHY:** Create stack from template

```json
{
    🔴 "StackId": "arn:aws:cloudformation:us-east-1:058264439561:stack/my-new-stack/67890"
}
```

### 🔵 `aws cloudformation create-stack --stack-name my-new-stack --template-url https://s3.amazonaws.com/bucket/template.yaml`
**WHY:** Create stack from S3 template

### 🔵 `aws cloudformation update-stack --stack-name my-stack --template-body file://new-template.yaml`
**WHY:** Update stack

### 🔵 `aws cloudformation delete-stack --stack-name my-new-stack`
**WHY:** 🔴 Delete stack and all resources

```
🟢 (No output = SUCCESS!)
```

---

## 🔵 CHANGE SET COMMANDS

### 🔵 `aws cloudformation create-change-set --stack-name my-stack --template-body file://new-template.yaml --change-set-name my-change`
**WHY:** Preview changes before applying

### 🔵 `aws cloudformation describe-change-set --change-set-name my-change --stack-name my-stack`
**WHY:** See what will change

### 🔵 `aws cloudformation execute-change-set --change-set-name my-change --stack-name my-stack`
**WHY:** Apply the changes

---

## 🔵 STACK RESOURCE COMMANDS

### 🔵 `aws cloudformation list-stack-resources --stack-name my-stack`
**WHY:** See all resources in stack

```json
{
    "StackResourceSummaries": [
        {
            🔴 "LogicalResourceId": "MyS3Bucket",
            🔴 "PhysicalResourceId": "my-bucket-name",
            🔴 "ResourceType": "AWS::S3::Bucket"
        }
    ]
}
```

---

# PART 16: ECR (ELASTIC CONTAINER REGISTRY)

## 🔵 REPOSITORY COMMANDS

### 🔵 `aws ecr describe-repositories`
**WHY:** List all ECR repositories

```json
{
    "repositories": [
        {
            🔴 "repositoryName": "my-app",
            🔴 "repositoryUri": "058264439561.dkr.ecr.us-east-1.amazonaws.com/my-app"
        }
    ]
}
```

### 🔵 `aws ecr create-repository --repository-name my-new-app`
**WHY:** Create new repository

```json
{
    "repository": {
        🔴 "repositoryName": "my-new-app",
        🔴 "repositoryUri": "058264439561.dkr.ecr.us-east-1.amazonaws.com/my-new-app"
    }
}
```

### 🔵 `aws ecr delete-repository --repository-name my-new-app`
**WHY:** Delete repository

```json
{
    "repository": {
        "repositoryName": "my-new-app"
    }
}
```

### 🔵 `aws ecr delete-repository --repository-name my-new-app --force`
**WHY:** 🔴 Force delete even if not empty

---

## 🔵 IMAGE COMMANDS

### 🔵 `aws ecr list-images --repository-name my-app`
**WHY:** List images in repository

```json
{
    "imageIds": [
        {
            🔴 "imageTag": "latest",
            🔴 "imageDigest": "sha256:1234567890abcdef"
        }
    ]
}
```

### 🔵 `aws ecr describe-images --repository-name my-app`
**WHY:** Get image details

### 🔵 `aws ecr batch-delete-image --repository-name my-app --image-ids imageTag=latest`
**WHY:** Delete specific image

### 🔵 `aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 058264439561.dkr.ecr.us-east-1.amazonaws.com`
**WHY:** 🔴 Login to ECR to push/pull images

---

# PART 17: ECS (ELASTIC CONTAINER SERVICE)

## 🔵 CLUSTER COMMANDS

### 🔵 `aws ecs list-clusters`
**WHY:** List all ECS clusters

```json
{
    "clusterArns": [
        🔴 "arn:aws:ecs:us-east-1:058264439561:cluster/my-cluster"
    ]
}
```

### 🔵 `aws ecs describe-clusters --clusters my-cluster`
**WHY:** Get cluster details

### 🔵 `aws ecs create-cluster --cluster-name my-new-cluster`
**WHY:** Create new cluster

### 🔵 `aws ecs delete-cluster --cluster my-cluster`
**WHY:** Delete cluster

---

## 🔵 SERVICE COMMANDS

### 🔵 `aws ecs list-services --cluster my-cluster`
**WHY:** List services in cluster

### 🔵 `aws ecs describe-services --cluster my-cluster --services my-service`
**WHY:** Get service details

### 🔵 `aws ecs create-service --cluster my-cluster --service-name my-service --task-definition my-task --desired-count 1`
**WHY:** Create new service

### 🔵 `aws ecs update-service --cluster my-cluster --service my-service --desired-count 2`
**WHY:** Scale service

### 🔵 `aws ecs delete-service --cluster my-cluster --service my-service`
**WHY:** Delete service

---

## 🔵 TASK COMMANDS

### 🔵 `aws ecs list-tasks --cluster my-cluster`
**WHY:** List running tasks

### 🔵 `aws ecs describe-tasks --cluster my-cluster --tasks arn:aws:ecs:us-east-1:058264439561:task/my-cluster/1234567890abcdef`
**WHY:** Get task details

### 🔵 `aws ecs run-task --cluster my-cluster --task-definition my-task`
**WHY:** Run one-off task

### 🔵 `aws ecs stop-task --cluster my-cluster --task arn:aws:ecs:us-east-1:058264439561:task/my-cluster/1234567890abcdef`
**WHY:** Stop task

---

## 🔵 TASK DEFINITION COMMANDS

### 🔵 `aws ecs list-task-definitions`
**WHY:** List all task definitions

### 🔵 `aws ecs describe-task-definition --task-definition my-task:1`
**WHY:** Get task definition details

### 🔵 `aws ecs register-task-definition --cli-input-json file://task-definition.json`
**WHY:** Create task definition

### 🔵 `aws ecs deregister-task-definition --task-definition my-task:1`
**WHY:** Delete task definition

---

# PART 18: EKS (ELASTIC KUBERNETES SERVICE)

## 🔵 CLUSTER COMMANDS

### 🔵 `aws eks list-clusters`
**WHY:** List all EKS clusters

```json
{
    "clusters": [
        🔴 "my-eks-cluster"
    ]
}
```

### 🔵 `aws eks describe-cluster --name my-eks-cluster`
**WHY:** Get cluster details

```json
{
    "cluster": {
        🔴 "name": "my-eks-cluster",
        🔴 "status": "ACTIVE",
        🔴 "endpoint": "https://1234567890.gr7.us-east-1.eks.amazonaws.com"
    }
}
```

### 🔵 `aws eks create-cluster --name my-new-cluster --role-arn arn:aws:iam::058264439561:role/eks-role --resources-vpc-config file://vpc-config.json`
**WHY:** Create new EKS cluster

### 🔵 `aws eks delete-cluster --name my-new-cluster`
**WHY:** Delete cluster

---

## 🔵 KUBECONFIG COMMANDS

### 🔵 `aws eks update-kubeconfig --name my-eks-cluster`
**WHY:** 🔴 Configure kubectl to connect to cluster

```
Updated context arn:aws:eks:us-east-1:058264439561:cluster/my-eks-cluster in C:\Users\.kube\config
```

### 🔵 `aws eks get-token --cluster-name my-eks-cluster`
**WHY:** Get authentication token

---

# PART 19: RDS (RELATIONAL DATABASE SERVICE)

## 🔵 INSTANCE COMMANDS

### 🔵 `aws rds describe-db-instances`
**WHY:** List all RDS databases

```json
{
    "DBInstances": [
        {
            🔴 "DBInstanceIdentifier": "my-database",
            🔴 "DBInstanceClass": "db.t3.micro",
            🔴 "Engine": "mysql",
            🔴 "DBInstanceStatus": "available",
            "Endpoint": {
                🔴 "Address": "my-database.123456789012.us-east-1.rds.amazonaws.com",
                🔴 "Port": 3306
            }
        }
    ]
}
```

### 🔵 `aws rds create-db-instance --db-instance-identifier my-new-db --db-instance-class db.t3.micro --engine mysql --master-username admin --master-user-password password123 --allocated-storage 20`
**WHY:** Create new database

```json
{
    "DBInstance": {
        🔴 "DBInstanceIdentifier": "my-new-db",
        🔴 "DBInstanceStatus": "creating"
    }
}
```

### 🔵 `aws rds delete-db-instance --db-instance-identifier my-new-db --skip-final-snapshot`
**WHY:** 🔴 Delete database

```json
{
    "DBInstance": {
        🔴 "DBInstanceStatus": "deleting"
    }
}
```

### 🔵 `aws rds reboot-db-instance --db-instance-identifier my-database`
**WHY:** Reboot database

### 🔵 `aws rds modify-db-instance --db-instance-identifier my-database --db-instance-class db.t3.large`
**WHY:** Change instance size

---

## 🔵 SNAPSHOT COMMANDS

### 🔵 `aws rds describe-db-snapshots`
**WHY:** List database snapshots

### 🔵 `aws rds create-db-snapshot --db-instance-identifier my-database --db-snapshot-identifier my-backup`
**WHY:** 🔴 Create backup

### 🔵 `aws rds delete-db-snapshot --db-snapshot-identifier my-backup`
**WHY:** Delete snapshot

### 🔵 `aws rds restore-db-instance-from-db-snapshot --db-instance-identifier restored-db --db-snapshot-identifier my-backup`
**WHY:** Restore from snapshot

---

# PART 20: ROUTE53 (DNS SERVICE)

## 🔵 HOSTED ZONE COMMANDS

### 🔵 `aws route53 list-hosted-zones`
**WHY:** List all DNS zones

```json
{
    "HostedZones": [
        {
            🔴 "Id": "/hostedzone/Z1234567890ABC",
            🔴 "Name": "example.com."
        }
    ]
}
```

### 🔵 `aws route53 create-hosted-zone --name example.com --caller-reference 2025-03-10-1`
**WHY:** Create new DNS zone

### 🔵 `aws route53 delete-hosted-zone --id /hostedzone/Z1234567890ABC`
**WHY:** Delete DNS zone

---

## 🔵 RECORD SET COMMANDS

### 🔵 `aws route53 list-resource-record-sets --hosted-zone-id /hostedzone/Z1234567890ABC`
**WHY:** List DNS records

```json
{
    "ResourceRecordSets": [
        {
            🔴 "Name": "example.com.",
            🔴 "Type": "A",
            🔴 "TTL": 300,
            "ResourceRecords": [
                {🔴 "Value": "192.0.2.1"}
            ]
        }
    ]
}
```

### 🔵 `aws route53 change-resource-record-sets --hosted-zone-id /hostedzone/Z1234567890ABC --change-batch file://records.json`
**WHY:** Add/update/delete DNS records

```json
{
    "ChangeInfo": {
        🔴 "Id": "/change/C1234567890",
        "Status": "PENDING"
    }
}
```

---

# PART 21: CLOUDFRONT (CDN)

## 🔵 DISTRIBUTION COMMANDS

### 🔵 `aws cloudfront list-distributions`
**WHY:** List all CloudFront distributions

```json
{
    "DistributionList": {
        "Items": [
            {
                🔴 "Id": "E1A2B3C4D5E6F7",
                🔴 "DomainName": "d123.cloudfront.net",
                🔴 "Status": "Deployed"
            }
        ]
    }
}
```

### 🔵 `aws cloudfront get-distribution --id E1A2B3C4D5E6F7`
**WHY:** Get distribution details

### 🔵 `aws cloudfront create-distribution --distribution-config file://config.json`
**WHY:** Create new CDN

### 🔵 `aws cloudfront delete-distribution --id E1A2B3C4D5E6F7 --if-match ETAG123`
**WHY:** Delete distribution

---

## 🔵 INVALIDATION COMMANDS

### 🔵 `aws cloudfront create-invalidation --distribution-id E1A2B3C4D5E6F7 --paths "/*"`
**WHY:** Clear CDN cache

```json
{
    "Invalidation": {
        🔴 "Id": "I1234567890",
        "Status": "InProgress"
    }
}
```

---

# PART 22: API GATEWAY

## 🔵 API COMMANDS

### 🔵 `aws apigateway get-rest-apis`
**WHY:** List all REST APIs

### 🔵 `aws apigateway create-rest-api --name "My API" --description "My API description"`
**WHY:** Create new API

### 🔵 `aws apigateway delete-rest-api --rest-api-id 1234567890`
**WHY:** Delete API

---

## 🔵 RESOURCE COMMANDS

### 🔵 `aws apigateway get-resources --rest-api-id 1234567890`
**WHY:** List API resources

### 🔵 `aws apigateway create-resource --rest-api-id 1234567890 --parent-id root --path-part users`
**WHY:** Create API endpoint

---

## 🔵 METHOD COMMANDS

### 🔵 `aws apigateway put-method --rest-api-id 1234567890 --resource-id abc123 --http-method GET --authorization-type NONE`
**WHY:** Add GET method to endpoint

### 🔵 `aws apigateway put-integration --rest-api-id 1234567890 --resource-id abc123 --http-method GET --type AWS --integration-http-method POST --uri arn:aws:lambda:us-east-1:058264439561:function:my-function`
**WHY:** Connect API to Lambda

---

# PART 23: GENERAL AWS COMMANDS

## 🔵 HELP COMMANDS
```bash
aws help
# WHY: Show general help

aws s3 help
# WHY: Show S3-specific help

aws ec2 describe-instances help
# WHY: Show help for specific command

aws help topics
# WHY: List all help topics
```

## 🔵 OUTPUT FORMATTING
```bash
aws s3 ls --output json
# WHY: JSON format (default)

aws s3 ls --output text
# WHY: Tab-separated text (good for scripting)

aws s3 ls --output table
# WHY: Human-readable table

aws s3 ls --no-cli-pager
# WHY: Disable pager (show all output at once)
```

## 🔵 QUERYING (JMESPath)
```bash
aws ec2 describe-instances --query 'Reservations[].Instances[].InstanceId'
# WHY: Get only instance IDs

aws ec2 describe-instances --query 'Reservations[].Instances[?State.Name==`running`].InstanceId'
# WHY: Get only running instances

aws s3api list-objects --bucket my-bucket --query 'Contents[?Size>`1000`].Key'
# WHY: Get files larger than 1000 bytes

aws iam list-users --query 'length(Users)'
# WHY: Count users
```

## 🔵 PAGINATION
```bash
aws s3api list-objects-v2 --bucket my-bucket --max-items 10
# WHY: Show only 10 items

aws s3api list-objects-v2 --bucket my-bucket --page-size 100
# WHY: Request 100 items per API call

aws s3api list-objects-v2 --bucket my-bucket --max-items 1000 --starting-token token
# WHY: Get next page of results
```

## 🔵 PROFILE USAGE
```bash
aws s3 ls --profile prod
# WHY: Use production profile

aws sts get-caller-identity --profile dev
# WHY: Check identity in dev profile

export AWS_PROFILE=prod
# WHY: Set default profile for all commands
```

## 🔵 REGION SPECIFICATION
```bash
aws s3 ls --region us-west-2
# WHY: Specify region

export AWS_DEFAULT_REGION=us-west-2
# WHY: Set default region
```

## 🔵 DEBUGGING
```bash
aws s3 ls --debug
# WHY: Show detailed debug info

aws s3 ls --no-verify-ssl
# WHY: Skip SSL verification (for testing)

aws s3 ls --cli-read-timeout 10
# WHY: Set timeout to 10 seconds
```

---

# 🏁 QUICK REFERENCE - MOST IMPORTANT COMMANDS FOR THE LAB!

| Step | Command | 🔴 Look For |
|------|---------|-------------|
| **1** | `aws sts get-caller-identity` | `"Arn": "user/BackupReader1"` |
| **2** | `aws iam get-policy-version` | `"iam:PutUserPolicy"` (Exploit!) |
| **3** | `aws iam put-user-policy` | No output = SUCCESS! |
| **4** | `aws s3 ls s3://securecopbakupbuk1/` | `key.txt`, `env.txt` |
| **5** | `aws s3 cp s3://securecopbakupbuk1/key.txt .` | Download success |
| **6** | `type key.txt` | `CustomerKey: ...` |
| **7** | `aws s3api get-object --key env.txt --sse-customer-key [KEY]` | `"SSECustomerAlgorithm": "AES256"` |
| **8** | `type env9.txt` | 🏁 `CWL{AWS_KMS_Key_chal!enge_Suc(es$}` |

---

# ✅ COMPLETE AWS CLI COMMAND REFERENCE CHECKLIST

- [ ] **Configuration**: `aws configure`, `aws configure list`
- [ ] **Identity**: `aws sts get-caller-identity`
- [ ] **IAM Users**: `list-users`, `get-user`, `create-user`, `delete-user`
- [ ] **IAM Policies**: `list-policies`, `get-policy`, `get-policy-version`
- [ ] **IAM Attachments**: `put-user-policy`, `list-attached-user-policies`
- [ ] **Access Keys**: `create-access-key`, `list-access-keys`
- [ ] **S3 Basic**: `ls`, `cp`, `mv`, `rm`, `sync`
- [ ] **S3 API**: `list-objects`, `get-object`, `put-object`
- [ ] **S3 Encryption**: `get-object` with `--sse-customer-key`
- [ ] **EC2 Instances**: `describe-instances`, `start-instances`, `stop-instances`
- [ ] **EC2 Security Groups**: `create-security-group`, `authorize-security-group-ingress`
- [ ] **EC2 Key Pairs**: `create-key-pair`, `describe-key-pairs`
- [ ] **EC2 Volumes**: `create-volume`, `attach-volume`, `create-snapshot`
- [ ] **KMS Keys**: `list-keys`, `describe-key`, `encrypt`, `decrypt`
- [ ] **Lambda**: `list-functions`, `invoke`
- [ ] **DynamoDB**: `list-tables`, `scan`, `get-item`
- [ ] **SQS**: `list-queues`, `send-message`, `receive-message`
- [ ] **SNS**: `list-topics`, `publish`, `subscribe`
- [ ] **CloudWatch Logs**: `describe-log-groups`, `get-log-events`
- [ ] **Secrets Manager**: `list-secrets`, `get-secret-value`
- [ ] **CloudFormation**: `list-stacks`, `create-stack`, `delete-stack`
- [ ] **Output Formatting**: `--output`, `--query`, `--max-items`

---

**This is the COMPLETE AWS CLI command reference with every command explained and color-coded important parts!** 🚀
