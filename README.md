# 🚀 AWS CLI ZERO TO HERO - COMPLETE GUIDE WITH LIVE EXAMPLES!

## 📋 EVERY AWS COMMAND WITH REAL OUTPUT EXAMPLES!

---

# PART 1: AWS CLI CONFIGURATION

## 1.1 INSTALL AWS CLI
```bash
# Check if AWS CLI is installed
aws --version
```
**🔍 OUTPUT:**
```
➡️ aws-cli/2.34.5 Python/3.13.11 Windows/11 exe/AMD64
```

## 1.2 CONFIGURE CREDENTIALS
```bash
aws configure
```
**🔍 INTERACTIVE INPUT:**
```
AWS Access Key ID [None]: ➡️ AKIAQ3EGUZME3BAXFV7D
AWS Secret Access Key [None]: ➡️ 2ETmQExURN6kBI0/2kClgcuTLyVBD5idkrJSFvsY
Default region name [None]: ➡️ us-east-1
Default output format [None]: ➡️ json
```

## 1.3 VIEW CONFIGURATION
```bash
aws configure list
```
**🔍 OUTPUT:**
```
      Name                    Value             Type    Location
      ----                    -----             ----    --------
   profile                <not set>             None    None
access_key     ****************FV7D shared-credentials-file
secret_key     ****************FvsY shared-credentials-file
    region                us-east-1      config-file    ~/.aws/config
```

## 1.4 VIEW CREDENTIALS FILE
```bash
type C:\Users\%USERNAME%\.aws\credentials
```
**🔍 OUTPUT:**
```
[default]
aws_access_key_id = ➡️ AKIAQ3EGUZME3BAXFV7D
aws_secret_access_key = ➡️ 2ETmQExURN6kBI0/2kClgcuTLyVBD5idkrJSFvsY
```

## 1.5 VIEW CONFIG FILE
```bash
type C:\Users\%USERNAME%\.aws\config
```
**🔍 OUTPUT:**
```
[default]
region = ➡️ us-east-1
output = ➡️ json
```

## 1.6 MULTIPLE PROFILES
```bash
aws configure --profile dev
aws configure --profile prod
```
**🔍 OUTPUT:** (Interactive - enter different credentials)

## 1.7 USE SPECIFIC PROFILE
```bash
aws sts get-caller-identity --profile dev
```

---

# PART 2: IDENTITY AND ACCESS MANAGEMENT (IAM)

## 2.1 GET CURRENT USER IDENTITY
```bash
aws sts get-caller-identity
```
**🔍 OUTPUT:**
```json
{
    "UserId": "➡️ AIDAQ3EGUZME24ILVB44T",
    "Account": "➡️ 058264439561",
    "Arn": "➡️ arn:aws:iam::058264439561:user/BackupReader1"
}
```

## 2.2 LIST ALL IAM USERS
```bash
aws iam list-users
```
**🔍 OUTPUT:**
```json
{
    "Users": [
        {
            "UserName": "➡️ BackupReader1",
            "UserId": "AIDAQ3EGUZME24ILVB44T",
            "Arn": "arn:aws:iam::058264439561:user/BackupReader1",
            "CreateDate": "2024-09-24T11:12:45+00:00"
        },
        {
            "UserName": "➡️ AdminUser",
            "UserId": "AIDAQ3EGUZME5HY78JKL",
            "Arn": "arn:aws:iam::058264439561:user/AdminUser",
            "CreateDate": "2024-01-15T09:30:22+00:00"
        }
    ]
}
```

## 2.3 GET SPECIFIC USER DETAILS
```bash
aws iam get-user --user-name BackupReader1
```
**🔍 OUTPUT:**
```json
{
    "User": {
        "UserName": "➡️ BackupReader1",
        "UserId": "AIDAQ3EGUZME24ILVB44T",
        "Arn": "arn:aws:iam::058264439561:user/BackupReader1",
        "CreateDate": "2024-09-24T11:12:45+00:00",
        "PasswordLastUsed": "2025-03-10T08:15:23+00:00",
        "PermissionsBoundary": {
            "PermissionsBoundaryArn": "➡️ arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy",
            "PermissionsBoundaryType": "Policy"
        }
    }
}
```

## 2.4 GET USER PERMISSIONS BOUNDARY
```bash
aws iam get-user --user-name BackupReader1 --query "User.PermissionsBoundary" --output text
```
**🔍 OUTPUT:**
```
➡️ arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy       Policy
```

## 2.5 LIST ALL IAM POLICIES
```bash
aws iam list-policies --scope Local --max-items 5
```
**🔍 OUTPUT:**
```json
{
    "Policies": [
        {
            "PolicyName": "➡️ PermissionBoundaryPolicy",
            "PolicyId": "ANPAQ3EGUZMEQVY32A5L4",
            "Arn": "➡️ arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy",
            "DefaultVersionId": "➡️ v3",
            "AttachmentCount": 0
        },
        {
            "PolicyName": "➡️ UserPolicy",
            "PolicyId": "ANPAQ3EGUZMEYBBY6PGTS",
            "Arn": "➡️ arn:aws:iam::058264439561:policy/UserPolicy",
            "DefaultVersionId": "➡️ v1",
            "AttachmentCount": 1
        }
    ]
}
```

## 2.6 GET POLICY DETAILS
```bash
aws iam get-policy --policy-arn arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy
```
**🔍 OUTPUT:**
```json
{
    "Policy": {
        "PolicyName": "➡️ PermissionBoundaryPolicy",
        "PolicyId": "ANPAQ3EGUZMEQVY32A5L4",
        "Arn": "➡️ arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy",
        "DefaultVersionId": "➡️ v3",
        "AttachmentCount": 0,
        "PermissionsBoundaryUsageCount": 1,
        "IsAttachable": true,
        "CreateDate": "2024-09-24T11:13:35+00:00",
        "UpdateDate": "2025-05-28T08:01:14+00:00"
    }
}
```

## 2.7 GET POLICY VERSION (VIEW ACTUAL PERMISSIONS)
```bash
aws iam get-policy-version --policy-arn arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy --version-id v3
```
**🔍 OUTPUT:**
```json
{
    "PolicyVersion": {
        "Document": {
            "Statement": [
                {
                    "Action": [
                        "➡️ s3:ListBucket",
                        "➡️ s3:GetObject"
                    ],
                    "Effect": "Allow",
                    "Resource": [
                        "➡️ arn:aws:s3:::securecopbakupbuk1",
                        "➡️ arn:aws:s3:::securecopbakupbuk1/*"
                    ]
                },
                {
                    "Action": [
                        "➡️ kms:Decrypt"
                    ],
                    "Effect": "Allow",
                    "Resource": "➡️ arn:aws:kms:us-east-1:058264439561:key/a7a251b3-c889-4dd1-9176-931c207c33d5"
                },
                {
                    "Action": [
                        "➡️ iam:PutUserPolicy"
                    ],
                    "Effect": "Allow",
                    "Resource": "➡️ arn:aws:iam::058264439561:user/BackupReader1"
                }
            ],
            "Version": "2012-10-17"
        },
        "VersionId": "v3",
        "IsDefaultVersion": true
    }
}
```

## 2.8 LIST ATTACHED USER POLICIES
```bash
aws iam list-attached-user-policies --user-name BackupReader1
```
**🔍 OUTPUT:**
```json
{
    "AttachedPolicies": [
        {
            "PolicyName": "➡️ UserPolicy",
            "PolicyArn": "➡️ arn:aws:iam::058264439561:policy/UserPolicy"
        }
    ]
}
```

## 2.9 CREATE AN INLINE POLICY
```bash
# First create policy.json file
echo '{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:*"
      ],
      "Resource": [
        "arn:aws:s3:::securecopbakupbuk1",
        "arn:aws:s3:::securecopbakupbuk1/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "kms:*",
      "Resource": "arn:aws:kms:us-east-1:058264439561:key/a7a251b3-c889-4dd1-9176-931c207c33d5"
    }
  ]
}' > policy.json
```

## 2.10 ATTACH INLINE POLICY TO USER
```bash
aws iam put-user-policy --user-name BackupReader1 --policy-name KMSFullAccessPolicy --policy-document file://policy.json
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 2.11 LIST INLINE USER POLICIES
```bash
aws iam list-user-policies --user-name BackupReader1
```
**🔍 OUTPUT:**
```json
{
    "PolicyNames": [
        "➡️ KMSFullAccessPolicy"
    ]
}
```

## 2.12 GET INLINE POLICY DETAILS
```bash
aws iam get-user-policy --user-name BackupReader1 --policy-name KMSFullAccessPolicy
```
**🔍 OUTPUT:**
```json
{
    "UserName": "BackupReader1",
    "PolicyName": "KMSFullAccessPolicy",
    "PolicyDocument": {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "➡️ s3:*"
                ],
                "Resource": [
                    "➡️ arn:aws:s3:::securecopbakupbuk1",
                    "➡️ arn:aws:s3:::securecopbakupbuk1/*"
                ]
            },
            {
                "Effect": "Allow",
                "Action": "➡️ kms:*",
                "Resource": "➡️ arn:aws:kms:us-east-1:058264439561:key/a7a251b3-c889-4dd1-9176-931c207c33d5"
            }
        ]
    }
}
```

## 2.13 LIST ALL IAM ROLES
```bash
aws iam list-roles --max-items 3
```
**🔍 OUTPUT:**
```json
{
    "Roles": [
        {
            "RoleName": "➡️ LambdaExecutionRole",
            "Arn": "arn:aws:iam::058264439561:role/LambdaExecutionRole",
            "CreateDate": "2024-06-15T10:20:33+00:00"
        },
        {
            "RoleName": "➡️ EC2InstanceRole",
            "Arn": "arn:aws:iam::058264439561:role/EC2InstanceRole",
            "CreateDate": "2024-07-22T14:35:12+00:00"
        }
    ]
}
```

## 2.14 LIST ALL IAM GROUPS
```bash
aws iam list-groups
```
**🔍 OUTPUT:**
```json
{
    "Groups": [
        {
            "GroupName": "➡️ Admins",
            "GroupId": "AGPAQ3EGUZME4X5Y6Z7A",
            "Arn": "arn:aws:iam::058264439561:group/Admins"
        },
        {
            "GroupName": "➡️ Developers",
            "GroupId": "AGPAQ3EGUZME8B9C0D1E",
            "Arn": "arn:aws:iam::058264439561:group/Developers"
        }
    ]
}
```

## 2.15 CREATE ACCESS KEY
```bash
aws iam create-access-key --user-name BackupReader1
```
**🔍 OUTPUT:**
```json
{
    "AccessKey": {
        "UserName": "BackupReader1",
        "AccessKeyId": "➡️ AKIAQ3EGUZME3NEWKEY12",
        "Status": "Active",
        "SecretAccessKey": "➡️ wJalrXUtnFEMI/K7MDENG/bPxRfiCYzEXAMPLEKEY",
        "CreateDate": "2025-03-10T10:25:33Z"
    }
}
```

## 2.16 LIST ACCESS KEYS
```bash
aws iam list-access-keys --user-name BackupReader1
```
**🔍 OUTPUT:**
```json
{
    "AccessKeyMetadata": [
        {
            "UserName": "BackupReader1",
            "AccessKeyId": "➡️ AKIAQ3EGUZME3BAXFV7D",
            "Status": "Active",
            "CreateDate": "2024-09-24T11:14:23Z"
        }
    ]
}
```

## 2.17 DELETE ACCESS KEY
```bash
aws iam delete-access-key --user-name BackupReader1 --access-key-id AKIAQ3EGUZME3BAXFV7D
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

---

# PART 3: S3 STORAGE SERVICE

## 3.1 LIST ALL S3 BUCKETS
```bash
aws s3 ls
```
**🔍 OUTPUT:**
```
2024-09-24 11:15:22 ➡️ securecopbakupbuk1
2024-01-10 09:30:45 ➡️ my-app-logs-2025
2024-03-18 14:22:33 ➡️ data-warehouse-prod
```

## 3.2 LIST BUCKET CONTENTS
```bash
aws s3 ls s3://securecopbakupbuk1/
```
**🔍 OUTPUT:**
```
                           PRE .BurpSuite/
2024-09-24 16:49:10        411 ➡️ Hospital+Patient+Records.zip
2024-09-24 16:48:40        411 ➡️ env.txt
2024-09-24 16:43:42        113 ➡️ key.txt
```

## 3.3 LIST WITH DETAILS (RECURSIVE)
```bash
aws s3 ls s3://securecopbakupbuk1/ --recursive --human-readable --summarize
```
**🔍 OUTPUT:**
```
2024-09-24 16:43:42  113 Bytes ➡️ key.txt
2024-09-24 16:48:40  411 Bytes ➡️ env.txt
2024-09-24 16:49:10  411 Bytes ➡️ Hospital+Patient+Records.zip
2024-09-24 16:50:22  1.2 KiB ➡️ .BurpSuite/config.json

Total Objects: 4
   Total Size: 2.1 KiB
```

## 3.4 UPLOAD FILE TO S3
```bash
echo "Hello AWS" > test.txt
aws s3 cp test.txt s3://securecopbakupbuk1/
```
**🔍 OUTPUT:**
```
upload: .\test.txt to s3://securecopbakupbuk1/test.txt
```

## 3.5 DOWNLOAD FILE FROM S3
```bash
aws s3 cp s3://securecopbakupbuk1/key.txt .
```
**🔍 OUTPUT:**
```
➡️ download: s3://securecopbakupbuk1/key.txt to .\key.txt
```

## 3.6 MOVE FILE IN S3
```bash
aws s3 mv s3://securecopbakupbuk1/test.txt s3://securecopbakupbuk1/backup/test.txt
```
**🔍 OUTPUT:**
```
move: s3://securecopbakupbuk1/test.txt to s3://securecopbakupbuk1/backup/test.txt
```

## 3.7 COPY FILE BETWEEN S3 LOCATIONS
```bash
aws s3 cp s3://securecopbakupbuk1/key.txt s3://securecopbakupbuk1/key-backup.txt
```
**🔍 OUTPUT:**
```
copy: s3://securecopbakupbuk1/key.txt to s3://securecopbakupbuk1/key-backup.txt
```

## 3.8 SYNC LOCAL FOLDER TO S3
```bash
mkdir myfolder
echo "data1" > myfolder/file1.txt
echo "data2" > myfolder/file2.txt
aws s3 sync myfolder/ s3://securecopbakupbuk1/mydata/
```
**🔍 OUTPUT:**
```
upload: myfolder\file1.txt to s3://securecopbakupbuk1/mydata/file1.txt
upload: myfolder\file2.txt to s3://securecopbakupbuk1/mydata/file2.txt
```

## 3.9 SYNC S3 TO LOCAL FOLDER
```bash
aws s3 sync s3://securecopbakupbuk1/ ./downloads/
```
**🔍 OUTPUT:**
```
download: s3://securecopbakupbuk1/key.txt to downloads\key.txt
download: s3://securecopbakupbuk1/env.txt to downloads\env.txt
```

## 3.10 DELETE FILE FROM S3
```bash
aws s3 rm s3://securecopbakupbuk1/test.txt
```
**🔍 OUTPUT:**
```
delete: s3://securecopbakupbuk1/test.txt
```

## 3.11 DELETE ALL FILES IN BUCKET
```bash
aws s3 rm s3://securecopbakupbuk1/ --recursive
```
**🔍 OUTPUT:**
```
delete: s3://securecopbakupbuk1/key.txt
delete: s3://securecopbakupbuk1/env.txt
delete: s3://securecopbakupbuk1/Hospital+Patient+Records.zip
```

## 3.12 CREATE NEW BUCKET
```bash
aws s3 mb s3://my-unique-bucket-name-12345 --region us-east-1
```
**🔍 OUTPUT:**
```
make_bucket: my-unique-bucket-name-12345
```

## 3.13 DELETE BUCKET
```bash
aws s3 rb s3://my-unique-bucket-name-12345
```
**🔍 OUTPUT:**
```
remove_bucket: my-unique-bucket-name-12345
```

## 3.14 DELETE NON-EMPTY BUCKET
```bash
aws s3 rb s3://securecopbakupbuk1 --force
```
**🔍 OUTPUT:**
```
delete: s3://securecopbakupbuk1/key.txt
delete: s3://securecopbakupbuk1/env.txt
remove_bucket: securecopbakupbuk1
```

---

# PART 4: S3API COMMANDS (ADVANCED S3)

## 4.1 CREATE BUCKET WITH API
```bash
aws s3api create-bucket --bucket my-api-bucket-123 --region us-east-1
```
**🔍 OUTPUT:**
```json
{
    "Location": "➡️ /my-api-bucket-123"
}
```

## 4.2 LIST BUCKETS WITH API
```bash
aws s3api list-buckets
```
**🔍 OUTPUT:**
```json
{
    "Buckets": [
        {
            "Name": "➡️ securecopbakupbuk1",
            "CreationDate": "2024-09-24T11:15:22+00:00"
        }
    ],
    "Owner": {
        "ID": "bcaf1ffd86f41161ca5fb16fd081034f"
    }
}
```

## 4.3 LIST OBJECTS WITH API
```bash
aws s3api list-objects --bucket securecopbakupbuk1
```
**🔍 OUTPUT:**
```json
{
    "Contents": [
        {
            "Key": "➡️ key.txt",
            "LastModified": "2024-09-24T16:43:42+00:00",
            "ETag": "\"d41d8cd98f00b204e9800998ecf8427e\"",
            "Size": 113,
            "StorageClass": "STANDARD"
        },
        {
            "Key": "➡️ env.txt",
            "LastModified": "2024-09-24T16:48:40+00:00",
            "ETag": "\"5d41402abc4b2a76b9719d911017c592\"",
            "Size": 411,
            "StorageClass": "STANDARD"
        }
    ]
}
```

## 4.4 LIST OBJECTS V2 (NEWER VERSION)
```bash
aws s3api list-objects-v2 --bucket securecopbakupbuk1 --max-items 2
```
**🔍 OUTPUT:**
```json
{
    "Contents": [
        {
            "Key": "➡️ .BurpSuite/",
            "Size": 0
        },
        {
            "Key": "➡️ Hospital+Patient+Records.zip",
            "Size": 411
        }
    ],
    "KeyCount": 2
}
```

## 4.5 UPLOAD OBJECT WITH API
```bash
aws s3api put-object --bucket securecopbakupbuk1 --key hello.txt --body hello.txt
```
**🔍 OUTPUT:**
```json
{
    "ETag": "\"5d41402abc4b2a76b9719d911017c592\""
}
```

## 4.6 GET OBJECT WITH API
```bash
aws s3api get-object --bucket securecopbakupbuk1 --key env.txt env-output.txt
```
**🔍 OUTPUT:**
```json
{
    "AcceptRanges": "bytes",
    "LastModified": "2024-09-24T16:48:40+00:00",
    "ContentLength": 411,
    "ETag": "\"5d41402abc4b2a76b9719d911017c592\"",
    "ContentType": "binary/octet-stream",
    "Metadata": {}
}
```

## 4.7 GET OBJECT WITH SSE-C (SERVER-SIDE ENCRYPTION WITH CUSTOMER KEY)
```bash
aws s3api get-object --bucket securecopbakupbuk1 --key env.txt ./env9.txt --sse-customer-algorithm AES256 --sse-customer-key ZhL8Zvaa3Xybf/sizoGwtrgKpxkxE46JJMiz1syVGQM= --sse-customer-key-md5 ESUzbd203tahLpxvA6LL4g==
```
**🔍 OUTPUT:**
```json
{
    "AcceptRanges": "bytes",
    "LastModified": "2024-09-24T16:48:40+00:00",
    "ContentLength": 411,
    "ETag": "\"5d41402abc4b2a76b9719d911017c592\"",
    "ContentType": "binary/octet-stream",
    "Metadata": {},
    "➡️ SSECustomerAlgorithm": "AES256",
    "➡️ SSECustomerKeyMD5": "ESUzbd203tahLpxvA6LL4g=="
}
```

## 4.8 DELETE OBJECT WITH API
```bash
aws s3api delete-object --bucket securecopbakupbuk1 --key hello.txt
```
**🔍 OUTPUT:**
```json
{
    "DeleteMarker": true
}
```

## 4.9 COPY OBJECT WITH API
```bash
aws s3api copy-object --bucket securecopbakupbuk1 --copy-source securecopbakupbuk1/key.txt --key key-copy.txt
```
**🔍 OUTPUT:**
```json
{
    "CopyObjectResult": {
        "ETag": "\"d41d8cd98f00b204e9800998ecf8427e\"",
        "LastModified": "2025-03-10T11:30:45+00:00"
    }
}
```

## 4.10 HEAD OBJECT (GET METADATA ONLY)
```bash
aws s3api head-object --bucket securecopbakupbuk1 --key key.txt
```
**🔍 OUTPUT:**
```json
{
    "AcceptRanges": "bytes",
    "LastModified": "2024-09-24T16:43:42+00:00",
    "ContentLength": 113,
    "ETag": "\"d41d8cd98f00b204e9800998ecf8427e\"",
    "ContentType": "binary/octet-stream",
    "Metadata": {}
}
```

## 4.11 SET BUCKET POLICY
```bash
# Create policy file
echo '{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::securecopbakupbuk1/*"
    }
  ]
}' > bucket-policy.json

aws s3api put-bucket-policy --bucket securecopbakupbuk1 --policy file://bucket-policy.json
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 4.12 GET BUCKET POLICY
```bash
aws s3api get-bucket-policy --bucket securecopbakupbuk1
```
**🔍 OUTPUT:**
```json
{
    "Policy": "{\n  \"Version\": \"2012-10-17\",\n  \"Statement\": [\n    {\n      \"Effect\": \"Allow\",\n      \"Principal\": \"*\",\n      \"Action\": \"s3:GetObject\",\n      \"Resource\": \"arn:aws:s3:::securecopbakupbuk1/*\"\n    }\n  ]\n}"
}
```

## 4.13 DELETE BUCKET POLICY
```bash
aws s3api delete-bucket-policy --bucket securecopbakupbuk1
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 4.14 SET BUCKET VERSIONING
```bash
aws s3api put-bucket-versioning --bucket securecopbakupbuk1 --versioning-configuration Status=Enabled
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 4.15 GET BUCKET VERSIONING STATUS
```bash
aws s3api get-bucket-versioning --bucket securecopbakupbuk1
```
**🔍 OUTPUT:**
```json
{
    "Status": "➡️ Enabled"
}
```

## 4.16 SET BUCKET ENCRYPTION
```bash
aws s3api put-bucket-encryption --bucket securecopbakupbuk1 --server-side-encryption-configuration '{
  "Rules": [
    {
      "ApplyServerSideEncryptionByDefault": {
        "SSEAlgorithm": "AES256"
      }
    }
  ]
}'
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 4.17 GET BUCKET ENCRYPTION
```bash
aws s3api get-bucket-encryption --bucket securecopbakupbuk1
```
**🔍 OUTPUT:**
```json
{
    "ServerSideEncryptionConfiguration": {
        "Rules": [
            {
                "ApplyServerSideEncryptionByDefault": {
                    "➡️ SSEAlgorithm": "AES256"
                }
            }
        ]
    }
}
```

## 4.18 SET BUCKET ACL (ACCESS CONTROL LIST)
```bash
aws s3api put-bucket-acl --bucket securecopbakupbuk1 --acl private
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 4.19 GET BUCKET ACL
```bash
aws s3api get-bucket-acl --bucket securecopbakupbuk1
```
**🔍 OUTPUT:**
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
            "Permission": "FULL_CONTROL"
        }
    ]
}
```

## 4.20 SET BUCKET CORS (CROSS-ORIGIN RESOURCE SHARING)
```bash
aws s3api put-bucket-cors --bucket securecopbakupbuk1 --cors-configuration '{
  "CORSRules": [
    {
      "AllowedOrigins": ["https://example.com"],
      "AllowedMethods": ["GET", "PUT"],
      "AllowedHeaders": ["*"]
    }
  ]
}'
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 4.21 GET BUCKET CORS
```bash
aws s3api get-bucket-cors --bucket securecopbakupbuk1
```
**🔍 OUTPUT:**
```json
{
    "CORSRules": [
        {
            "AllowedOrigins": [
                "➡️ https://example.com"
            ],
            "AllowedMethods": [
                "➡️ GET",
                "➡️ PUT"
            ],
            "AllowedHeaders": [
                "➡️ *"
            ]
        }
    ]
}
```

## 4.22 SET BUCKET LIFECYCLE
```bash
aws s3api put-bucket-lifecycle-configuration --bucket securecopbakupbuk1 --lifecycle-configuration '{
  "Rules": [
    {
      "Id": "Delete old logs",
      "Status": "Enabled",
      "Prefix": "logs/",
      "Expiration": {
        "Days": 30
      }
    }
  ]
}'
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 4.23 GET BUCKET LOCATION
```bash
aws s3api get-bucket-location --bucket securecopbakupbuk1
```
**🔍 OUTPUT:**
```json
{
    "LocationConstraint": "➡️ us-east-1"
}
```

## 4.24 GET OBJECT TAGGING
```bash
aws s3api get-object-tagging --bucket securecopbakupbuk1 --key key.txt
```
**🔍 OUTPUT:**
```json
{
    "TagSet": [
        {
            "Key": "➡️ Confidential",
            "Value": "➡️ True"
        }
    ]
}
```

## 4.25 PUT OBJECT TAGGING
```bash
aws s3api put-object-tagging --bucket securecopbakupbuk1 --key key.txt --tagging '{
  "TagSet": [
    {
      "Key": "Project",
      "Value": "SecurityLab"
    }
  ]
}'
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

---

# PART 5: EC2 COMPUTE SERVICE

## 5.1 DESCRIBE EC2 INSTANCES
```bash
aws ec2 describe-instances
```
**🔍 OUTPUT:**
```json
{
    "Reservations": [
        {
            "Instances": [
                {
                    "InstanceId": "➡️ i-1234567890abcdef0",
                    "InstanceType": "➡️ t2.micro",
                    "State": {
                        "Name": "➡️ running"
                    },
                    "ImageId": "ami-0c55b159cbfafe1f0",
                    "PrivateIpAddress": "172.31.45.123",
                    "PublicIpAddress": "54.123.45.67",
                    "KeyName": "my-key-pair",
                    "LaunchTime": "2025-02-15T10:30:25+00:00"
                }
            ]
        }
    ]
}
```

## 5.2 DESCRIBE INSTANCES WITH FILTER
```bash
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" --query "Reservations[].Instances[].InstanceId"
```
**🔍 OUTPUT:**
```json
[
    "➡️ i-1234567890abcdef0",
    "➡️ i-0987654321fedcba0"
]
```

## 5.3 LAUNCH NEW EC2 INSTANCE
```bash
aws ec2 run-instances --image-id ami-0c55b159cbfafe1f0 --instance-type t2.micro --key-name my-key-pair --security-group-ids sg-12345678 --subnet-id subnet-12345678 --count 1
```
**🔍 OUTPUT:**
```json
{
    "Instances": [
        {
            "InstanceId": "➡️ i-1234567890abcdef1",
            "InstanceType": "t2.micro",
            "State": {
                "Name": "➡️ pending"
            },
            "LaunchTime": "2025-03-10T12:15:30+00:00"
        }
    ]
}
```

## 5.4 START EC2 INSTANCE
```bash
aws ec2 start-instances --instance-ids i-1234567890abcdef1
```
**🔍 OUTPUT:**
```json
{
    "StartingInstances": [
        {
            "InstanceId": "➡️ i-1234567890abcdef1",
            "CurrentState": {
                "Name": "➡️ starting"
            },
            "PreviousState": {
                "Name": "➡️ stopped"
            }
        }
    ]
}
```

## 5.5 STOP EC2 INSTANCE
```bash
aws ec2 stop-instances --instance-ids i-1234567890abcdef1
```
**🔍 OUTPUT:**
```json
{
    "StoppingInstances": [
        {
            "InstanceId": "➡️ i-1234567890abcdef1",
            "CurrentState": {
                "Name": "➡️ stopping"
            },
            "PreviousState": {
                "Name": "➡️ running"
            }
        }
    ]
}
```

## 5.6 REBOOT EC2 INSTANCE
```bash
aws ec2 reboot-instances --instance-ids i-1234567890abcdef1
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 5.7 TERMINATE EC2 INSTANCE
```bash
aws ec2 terminate-instances --instance-ids i-1234567890abcdef1
```
**🔍 OUTPUT:**
```json
{
    "TerminatingInstances": [
        {
            "InstanceId": "➡️ i-1234567890abcdef1",
            "CurrentState": {
                "Name": "➡️ shutting-down"
            },
            "PreviousState": {
                "Name": "➡️ running"
            }
        }
    ]
}
```

## 5.8 CREATE KEY PAIR
```bash
aws ec2 create-key-pair --key-name my-new-key --query 'KeyMaterial' --output text > my-new-key.pem
```
**🔍 OUTPUT:** (Saves private key to file)
```
➡️ (Key material saved to my-new-key.pem)
```

## 5.9 LIST KEY PAIRS
```bash
aws ec2 describe-key-pairs
```
**🔍 OUTPUT:**
```json
{
    "KeyPairs": [
        {
            "KeyName": "➡️ my-key-pair",
            "KeyFingerprint": "12:34:56:78:90:ab:cd:ef:12:34:56:78:90:ab:cd:ef"
        }
    ]
}
```

## 5.10 DELETE KEY PAIR
```bash
aws ec2 delete-key-pair --key-name my-key-pair
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 5.11 CREATE SECURITY GROUP
```bash
aws ec2 create-security-group --group-name my-security-group --description "My security group" --vpc-id vpc-12345678
```
**🔍 OUTPUT:**
```json
{
    "GroupId": "➡️ sg-1234567890abcdef0"
}
```

## 5.12 DESCRIBE SECURITY GROUPS
```bash
aws ec2 describe-security-groups --group-ids sg-1234567890abcdef0
```
**🔍 OUTPUT:**
```json
{
    "SecurityGroups": [
        {
            "GroupId": "sg-1234567890abcdef0",
            "GroupName": "my-security-group",
            "Description": "My security group",
            "VpcId": "vpc-12345678",
            "IpPermissions": []
        }
    ]
}
```

## 5.13 AUTHORIZE SECURITY GROUP INGRESS (ALLOW SSH)
```bash
aws ec2 authorize-security-group-ingress --group-id sg-1234567890abcdef0 --protocol tcp --port 22 --cidr 0.0.0.0/0
```
**🔍 OUTPUT:**
```json
{
    "Return": true,
    "SecurityGroupRules": [
        {
            "SecurityGroupRuleId": "sgr-1234567890abcdef0",
            "GroupId": "sg-1234567890abcdef0",
            "CidrIpv4": "0.0.0.0/0",
            "FromPort": 22,
            "ToPort": 22,
            "IpProtocol": "tcp"
        }
    ]
}
```

## 5.14 AUTHORIZE SECURITY GROUP INGRESS (ALLOW HTTP)
```bash
aws ec2 authorize-security-group-ingress --group-id sg-1234567890abcdef0 --protocol tcp --port 80 --cidr 0.0.0.0/0
```
**🔍 OUTPUT:**
```json
{
    "Return": true,
    "SecurityGroupRules": [
        {
            "SecurityGroupRuleId": "sgr-0987654321fedcba0",
            "GroupId": "sg-1234567890abcdef0",
            "CidrIpv4": "0.0.0.0/0",
            "FromPort": 80,
            "ToPort": 80,
            "IpProtocol": "tcp"
        }
    ]
}
```

## 5.15 REVOKE SECURITY GROUP INGRESS
```bash
aws ec2 revoke-security-group-ingress --group-id sg-1234567890abcdef0 --protocol tcp --port 22 --cidr 0.0.0.0/0
```
**🔍 OUTPUT:**
```json
{
    "Return": true
}
```

## 5.16 CREATE SECURITY GROUP EGRESS RULE
```bash
aws ec2 authorize-security-group-egress --group-id sg-1234567890abcdef0 --protocol tcp --port 443 --cidr 0.0.0.0/0
```
**🔍 OUTPUT:**
```json
{
    "Return": true
}
```

## 5.17 DELETE SECURITY GROUP
```bash
aws ec2 delete-security-group --group-id sg-1234567890abcdef0
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 5.18 ALLOCATE ELASTIC IP
```bash
aws ec2 allocate-address --domain vpc
```
**🔍 OUTPUT:**
```json
{
    "PublicIp": "➡️ 54.123.45.68",
    "AllocationId": "➡️ eipalloc-12345678",
    "Domain": "vpc"
}
```

## 5.19 ASSOCIATE ELASTIC IP
```bash
aws ec2 associate-address --instance-id i-1234567890abcdef1 --allocation-id eipalloc-12345678
```
**🔍 OUTPUT:**
```json
{
    "AssociationId": "➡️ eipassoc-12345678"
}
```

## 5.20 DISASSOCIATE ELASTIC IP
```bash
aws ec2 disassociate-address --association-id eipassoc-12345678
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 5.21 RELEASE ELASTIC IP
```bash
aws ec2 release-address --allocation-id eipalloc-12345678
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 5.22 CREATE VOLUME (EBS)
```bash
aws ec2 create-volume --availability-zone us-east-1a --size 10 --volume-type gp2
```
**🔍 OUTPUT:**
```json
{
    "VolumeId": "➡️ vol-1234567890abcdef0",
    "Size": 10,
    "State": "creating",
    "AvailabilityZone": "us-east-1a",
    "VolumeType": "gp2",
    "CreateTime": "2025-03-10T13:45:22+00:00"
}
```

## 5.23 DESCRIBE VOLUMES
```bash
aws ec2 describe-volumes --volume-ids vol-1234567890abcdef0
```
**🔍 OUTPUT:**
```json
{
    "Volumes": [
        {
            "VolumeId": "vol-1234567890abcdef0",
            "Size": 10,
            "State": "available",
            "AvailabilityZone": "us-east-1a",
            "VolumeType": "gp2"
        }
    ]
}
```

## 5.24 ATTACH VOLUME TO INSTANCE
```bash
aws ec2 attach-volume --volume-id vol-1234567890abcdef0 --instance-id i-1234567890abcdef1 --device /dev/sdf
```
**🔍 OUTPUT:**
```json
{
    "VolumeId": "vol-1234567890abcdef0",
    "InstanceId": "i-1234567890abcdef1",
    "Device": "/dev/sdf",
    "State": "attaching"
}
```

## 5.25 DETACH VOLUME
```bash
aws ec2 detach-volume --volume-id vol-1234567890abcdef0
```
**🔍 OUTPUT:**
```json
{
    "VolumeId": "vol-1234567890abcdef0",
    "InstanceId": "i-1234567890abcdef1",
    "Device": "/dev/sdf",
    "State": "detaching"
}
```

## 5.26 DELETE VOLUME
```bash
aws ec2 delete-volume --volume-id vol-1234567890abcdef0
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 5.27 CREATE SNAPSHOT
```bash
aws ec2 create-snapshot --volume-id vol-1234567890abcdef0 --description "Backup snapshot"
```
**🔍 OUTPUT:**
```json
{
    "SnapshotId": "➡️ snap-1234567890abcdef0",
    "VolumeId": "vol-1234567890abcdef0",
    "State": "pending",
    "StartTime": "2025-03-10T14:20:15+00:00",
    "Description": "Backup snapshot"
}
```

## 5.28 DESCRIBE SNAPSHOTS
```bash
aws ec2 describe-snapshots --snapshot-ids snap-1234567890abcdef0
```
**🔍 OUTPUT:**
```json
{
    "Snapshots": [
        {
            "SnapshotId": "snap-1234567890abcdef0",
            "VolumeId": "vol-1234567890abcdef0",
            "State": "completed",
            "StartTime": "2025-03-10T14:20:15+00:00",
            "Description": "Backup snapshot"
        }
    ]
}
```

## 5.29 DELETE SNAPSHOT
```bash
aws ec2 delete-snapshot --snapshot-id snap-1234567890abcdef0
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 5.30 CREATE TAGS
```bash
aws ec2 create-tags --resources i-1234567890abcdef1 --tags Key=Name,Value=MyWebServer Key=Environment,Value=Production
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 5.31 DESCRIBE TAGS
```bash
aws ec2 describe-tags --filters "Name=resource-id,Values=i-1234567890abcdef1"
```
**🔍 OUTPUT:**
```json
{
    "Tags": [
        {
            "Key": "➡️ Name",
            "Value": "➡️ MyWebServer",
            "ResourceId": "i-1234567890abcdef1"
        },
        {
            "Key": "➡️ Environment",
            "Value": "➡️ Production",
            "ResourceId": "i-1234567890abcdef1"
        }
    ]
}
```

## 5.32 DESCRIBE VPCs
```bash
aws ec2 describe-vpcs
```
**🔍 OUTPUT:**
```json
{
    "Vpcs": [
        {
            "VpcId": "➡️ vpc-12345678",
            "CidrBlock": "10.0.0.0/16",
            "State": "available",
            "IsDefault": true
        }
    ]
}
```

## 5.33 DESCRIBE SUBNETS
```bash
aws ec2 describe-subnets
```
**🔍 OUTPUT:**
```json
{
    "Subnets": [
        {
            "SubnetId": "➡️ subnet-12345678",
            "VpcId": "vpc-12345678",
            "CidrBlock": "10.0.1.0/24",
            "AvailabilityZone": "us-east-1a"
        }
    ]
}
```

## 5.34 DESCRIBE AMIs (Amazon Machine Images)
```bash
aws ec2 describe-images --owners amazon --filters "Name=name,Values=amzn2-ami-hvm-2.0.*" --max-items 5
```
**🔍 OUTPUT:**
```json
{
    "Images": [
        {
            "ImageId": "➡️ ami-0c55b159cbfafe1f0",
            "Name": "amzn2-ami-hvm-2.0.20250301-x86_64-gp2",
            "Description": "Amazon Linux 2 AMI"
        }
    ]
}
```

---

# PART 6: KMS (KEY MANAGEMENT SERVICE)

## 6.1 LIST KMS KEYS
```bash
aws kms list-keys --max-items 5
```
**🔍 OUTPUT:**
```json
{
    "Keys": [
        {
            "KeyId": "➡️ a7a251b3-c889-4dd1-9176-931c207c33d5",
            "KeyArn": "➡️ arn:aws:kms:us-east-1:058264439561:key/a7a251b3-c889-4dd1-9176-931c207c33d5"
        }
    ]
}
```

## 6.2 DESCRIBE KMS KEY
```bash
aws kms describe-key --key-id arn:aws:kms:us-east-1:058264439561:key/a7a251b3-c889-4dd1-9176-931c207c33d5
```
**🔍 OUTPUT:**
```json
{
    "KeyMetadata": {
        "KeyId": "a7a251b3-c889-4dd1-9176-931c207c33d5",
        "Arn": "arn:aws:kms:us-east-1:058264439561:key/a7a251b3-c889-4dd1-9176-931c207c33d5",
        "CreationDate": "2024-09-15T10:30:22+00:00",
        "Enabled": true,
        "Description": "Key for S3 encryption",
        "KeyState": "Enabled",
        "KeyUsage": "ENCRYPT_DECRYPT",
        "CustomerMasterKeySpec": "SYMMETRIC_DEFAULT"
    }
}
```

## 6.3 LIST KMS ALIASES
```bash
aws kms list-aliases
```
**🔍 OUTPUT:**
```json
{
    "Aliases": [
        {
            "AliasName": "➡️ alias/s3-key",
            "AliasArn": "arn:aws:kms:us-east-1:058264439561:alias/s3-key",
            "TargetKeyId": "a7a251b3-c889-4dd1-9176-931c207c33d5"
        }
    ]
}
```

## 6.4 CREATE KMS KEY
```bash
aws kms create-key --description "My new KMS key"
```
**🔍 OUTPUT:**
```json
{
    "KeyMetadata": {
        "KeyId": "➡️ b8b362c4-d990-5ee2-8277-8a9c2a1b3d4e",
        "Arn": "arn:aws:kms:us-east-1:058264439561:key/b8b362c4-d990-5ee2-8277-8a9c2a1b3d4e",
        "CreationDate": "2025-03-10T15:30:45+00:00",
        "Enabled": true,
        "KeyState": "Enabled"
    }
}
```

## 6.5 CREATE KMS ALIAS
```bash
aws kms create-alias --alias-name alias/my-new-key --target-key-id b8b362c4-d990-5ee2-8277-8a9c2a1b3d4e
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 6.6 ENABLE KMS KEY
```bash
aws kms enable-key --key-id a7a251b3-c889-4dd1-9176-931c207c33d5
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 6.7 DISABLE KMS KEY
```bash
aws kms disable-key --key-id a7a251b3-c889-4dd1-9176-931c207c33d5
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 6.8 SCHEDULE KEY DELETION
```bash
aws kms schedule-key-deletion --key-id a7a251b3-c889-4dd1-9176-931c207c33d5 --pending-window-in-days 7
```
**🔍 OUTPUT:**
```json
{
    "KeyId": "a7a251b3-c889-4dd1-9176-931c207c33d5",
    "DeletionDate": "2025-03-17T15:45:22+00:00"
}
```

## 6.9 CANCEL KEY DELETION
```bash
aws kms cancel-key-deletion --key-id a7a251b3-c889-4dd1-9176-931c207c33d5
```
**🔍 OUTPUT:**
```json
{
    "KeyId": "a7a251b3-c889-4dd1-9176-931c207c33d5"
}
```

## 6.10 GET KEY POLICY
```bash
aws kms get-key-policy --key-id a7a251b3-c889-4dd1-9176-931c207c33d5 --policy-name default
```
**🔍 OUTPUT:**
```json
{
    "Policy": "{\n  \"Version\": \"2012-10-17\",\n  \"Statement\": [\n    {\n      \"Effect\": \"Allow\",\n      \"Principal\": {\n        \"AWS\": \"arn:aws:iam::058264439561:user/BackupReader1\"\n      },\n      \"Action\": \"kms:Decrypt\",\n      \"Resource\": \"*\"\n    }\n  ]\n}"
}
```

---

# PART 7: LAMBDA (SERVERLESS)

## 7.1 LIST LAMBDA FUNCTIONS
```bash
aws lambda list-functions --max-items 5
```
**🔍 OUTPUT:**
```json
{
    "Functions": [
        {
            "FunctionName": "➡️ my-function",
            "FunctionArn": "arn:aws:lambda:us-east-1:058264439561:function:my-function",
            "Runtime": "nodejs18.x",
            "Role": "arn:aws:iam::058264439561:role/lambda-role",
            "Handler": "index.handler",
            "CodeSize": 12345,
            "LastModified": "2025-02-20T14:30:22+00:00"
        }
    ]
}
```

## 7.2 INVOKE LAMBDA FUNCTION
```bash
aws lambda invoke --function-name my-function output.txt
```
**🔍 OUTPUT:**
```json
{
    "StatusCode": 200,
    "ExecutedVersion": "$LATEST"
}
```

## 7.3 CREATE LAMBDA FUNCTION
```bash
# First create a ZIP file with your code
zip function.zip index.js

aws lambda create-function --function-name my-new-function --runtime nodejs18.x --role arn:aws:iam::058264439561:role/lambda-role --handler index.handler --zip-file fileb://function.zip
```
**🔍 OUTPUT:**
```json
{
    "FunctionName": "my-new-function",
    "FunctionArn": "arn:aws:lambda:us-east-1:058264439561:function:my-new-function",
    "Runtime": "nodejs18.x",
    "Role": "arn:aws:iam::058264439561:role/lambda-role"
}
```

## 7.4 DELETE LAMBDA FUNCTION
```bash
aws lambda delete-function --function-name my-new-function
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

---

# PART 8: CLOUDWATCH (MONITORING)

## 8.1 LIST METRICS
```bash
aws cloudwatch list-metrics --namespace AWS/EC2 --max-items 5
```
**🔍 OUTPUT:**
```json
{
    "Metrics": [
        {
            "Namespace": "AWS/EC2",
            "MetricName": "➡️ CPUUtilization",
            "Dimensions": [
                {
                    "Name": "InstanceId",
                    "Value": "i-1234567890abcdef0"
                }
            ]
        }
    ]
}
```

## 8.2 GET METRIC STATISTICS
```bash
aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization --dimensions Name=InstanceId,Value=i-1234567890abcdef0 --start-time 2025-03-09T00:00:00Z --end-time 2025-03-10T00:00:00Z --period 3600 --statistics Average
```
**🔍 OUTPUT:**
```json
{
    "Datapoints": [
        {
            "Timestamp": "2025-03-09T10:00:00Z",
            "Average": 12.5,
            "Unit": "Percent"
        }
    ]
}
```

## 8.3 LIST ALARMS
```bash
aws cloudwatch describe-alarms
```
**🔍 OUTPUT:**
```json
{
    "MetricAlarms": [
        {
            "AlarmName": "➡️ High CPU Alarm",
            "AlarmArn": "arn:aws:cloudwatch:us-east-1:058264439561:alarm:High CPU Alarm",
            "MetricName": "CPUUtilization",
            "StateValue": "OK"
        }
    ]
}
```

---

# PART 9: CLOUDFORMATION (INFRASTRUCTURE AS CODE)

## 9.1 LIST STACKS
```bash
aws cloudformation list-stacks
```
**🔍 OUTPUT:**
```json
{
    "StackSummaries": [
        {
            "StackName": "➡️ my-stack",
            "StackId": "arn:aws:cloudformation:us-east-1:058264439561:stack/my-stack/12345",
            "StackStatus": "➡️ CREATE_COMPLETE"
        }
    ]
}
```

## 9.2 CREATE STACK
```bash
aws cloudformation create-stack --stack-name my-new-stack --template-body file://template.yaml
```
**🔍 OUTPUT:**
```json
{
    "StackId": "arn:aws:cloudformation:us-east-1:058264439561:stack/my-new-stack/67890"
}
```

## 9.3 DELETE STACK
```bash
aws cloudformation delete-stack --stack-name my-new-stack
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

---

# PART 10: DYNAMODB (NoSQL DATABASE)

## 10.1 LIST TABLES
```bash
aws dynamodb list-tables
```
**🔍 OUTPUT:**
```json
{
    "TableNames": [
        "➡️ Users",
        "➡️ Orders"
    ]
}
```

## 10.2 SCAN TABLE
```bash
aws dynamodb scan --table-name Users
```
**🔍 OUTPUT:**
```json
{
    "Items": [
        {
            "UserId": {
                "S": "➡️ user123"
            },
            "Name": {
                "S": "➡️ John Doe"
            },
            "Email": {
                "S": "➡️ john@example.com"
            }
        }
    ],
    "Count": 1
}
```

## 10.3 GET ITEM
```bash
aws dynamodb get-item --table-name Users --key '{"UserId":{"S":"user123"}}'
```
**🔍 OUTPUT:**
```json
{
    "Item": {
        "UserId": {
            "S": "user123"
        },
        "Name": {
            "S": "John Doe"
        },
        "Email": {
            "S": "john@example.com"
        }
    }
}
```

## 10.4 PUT ITEM
```bash
aws dynamodb put-item --table-name Users --item '{"UserId":{"S":"user456"},"Name":{"S":"Jane Smith"},"Email":{"S":"jane@example.com"}}'
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 10.5 DELETE ITEM
```bash
aws dynamodb delete-item --table-name Users --key '{"UserId":{"S":"user456"}}'
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

---

# PART 11: RDS (RELATIONAL DATABASE SERVICE)

## 11.1 LIST RDS INSTANCES
```bash
aws rds describe-db-instances
```
**🔍 OUTPUT:**
```json
{
    "DBInstances": [
        {
            "DBInstanceIdentifier": "➡️ my-database",
            "DBInstanceClass": "db.t3.micro",
            "Engine": "mysql",
            "DBInstanceStatus": "available",
            "Endpoint": {
                "Address": "my-database.123456789012.us-east-1.rds.amazonaws.com",
                "Port": 3306
            }
        }
    ]
}
```

## 11.2 CREATE RDS INSTANCE
```bash
aws rds create-db-instance --db-instance-identifier my-new-db --db-instance-class db.t3.micro --engine mysql --master-username admin --master-user-password password123 --allocated-storage 20
```
**🔍 OUTPUT:**
```json
{
    "DBInstance": {
        "DBInstanceIdentifier": "my-new-db",
        "DBInstanceClass": "db.t3.micro",
        "Engine": "mysql",
        "DBInstanceStatus": "creating"
    }
}
```

## 11.3 DELETE RDS INSTANCE
```bash
aws rds delete-db-instance --db-instance-identifier my-new-db --skip-final-snapshot
```
**🔍 OUTPUT:**
```json
{
    "DBInstance": {
        "DBInstanceIdentifier": "my-new-db",
        "DBInstanceStatus": "deleting"
    }
}
```

---

# PART 12: SQS (SIMPLE QUEUE SERVICE)

## 12.1 LIST QUEUES
```bash
aws sqs list-queues
```
**🔍 OUTPUT:**
```json
{
    "QueueUrls": [
        "➡️ https://sqs.us-east-1.amazonaws.com/058264439561/my-queue"
    ]
}
```

## 12.2 CREATE QUEUE
```bash
aws sqs create-queue --queue-name my-new-queue
```
**🔍 OUTPUT:**
```json
{
    "QueueUrl": "➡️ https://sqs.us-east-1.amazonaws.com/058264439561/my-new-queue"
}
```

## 12.3 SEND MESSAGE
```bash
aws sqs send-message --queue-url https://sqs.us-east-1.amazonaws.com/058264439561/my-queue --message-body "Hello world"
```
**🔍 OUTPUT:**
```json
{
    "MessageId": "➡️ 12345678-1234-1234-1234-123456789012",
    "MD5OfMessageBody": "5d41402abc4b2a76b9719d911017c592"
}
```

## 12.4 RECEIVE MESSAGE
```bash
aws sqs receive-message --queue-url https://sqs.us-east-1.amazonaws.com/058264439561/my-queue
```
**🔍 OUTPUT:**
```json
{
    "Messages": [
        {
            "MessageId": "12345678-1234-1234-1234-123456789012",
            "ReceiptHandle": "AQEBwJnK...",
            "MD5OfBody": "5d41402abc4b2a76b9719d911017c592",
            "Body": "➡️ Hello world"
        }
    ]
}
```

## 12.5 DELETE MESSAGE
```bash
aws sqs delete-message --queue-url https://sqs.us-east-1.amazonaws.com/058264439561/my-queue --receipt-handle "AQEBwJnK..."
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

## 12.6 DELETE QUEUE
```bash
aws sqs delete-queue --queue-url https://sqs.us-east-1.amazonaws.com/058264439561/my-new-queue
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

---

# PART 13: SNS (SIMPLE NOTIFICATION SERVICE)

## 13.1 LIST TOPICS
```bash
aws sns list-topics
```
**🔍 OUTPUT:**
```json
{
    "Topics": [
        {
            "TopicArn": "➡️ arn:aws:sns:us-east-1:058264439561:my-topic"
        }
    ]
}
```

## 13.2 CREATE TOPIC
```bash
aws sns create-topic --name my-new-topic
```
**🔍 OUTPUT:**
```json
{
    "TopicArn": "➡️ arn:aws:sns:us-east-1:058264439561:my-new-topic"
}
```

## 13.3 SUBSCRIBE TO TOPIC
```bash
aws sns subscribe --topic-arn arn:aws:sns:us-east-1:058264439561:my-new-topic --protocol email --notification-endpoint user@example.com
```
**🔍 OUTPUT:**
```json
{
    "SubscriptionArn": "➡️ pending"
}
```

## 13.4 PUBLISH TO TOPIC
```bash
aws sns publish --topic-arn arn:aws:sns:us-east-1:058264439561:my-new-topic --message "Hello subscribers" --subject "Test Message"
```
**🔍 OUTPUT:**
```json
{
    "MessageId": "➡️ 12345678-1234-1234-1234-123456789012"
}
```

## 13.5 DELETE TOPIC
```bash
aws sns delete-topic --topic-arn arn:aws:sns:us-east-1:058264439561:my-new-topic
```
**🔍 OUTPUT:** (No output = SUCCESS!)
```
➡️ (No output - command successful)
```

---

# PART 14: CLOUDFRONT (CDN)

## 14.1 LIST DISTRIBUTIONS
```bash
aws cloudfront list-distributions --max-items 5
```
**🔍 OUTPUT:**
```json
{
    "DistributionList": {
        "Items": [
            {
                "Id": "➡️ E1A2B3C4D5E6F7",
                "ARN": "arn:aws:cloudfront::058264439561:distribution/E1A2B3C4D5E6F7",
                "Status": "Deployed",
                "DomainName": "➡️ d123.cloudfront.net"
            }
        ]
    }
}
```

---

# PART 15: ROUTE53 (DNS)

## 15.1 LIST HOSTED ZONES
```bash
aws route53 list-hosted-zones
```
**🔍 OUTPUT:**
```json
{
    "HostedZones": [
        {
            "Id": "/hostedzone/Z1234567890ABC",
            "Name": "➡️ example.com.",
            "CallerReference": "12345"
        }
    ]
}
```

## 15.2 LIST RECORD SETS
```bash
aws route53 list-resource-record-sets --hosted-zone-id /hostedzone/Z1234567890ABC
```
**🔍 OUTPUT:**
```json
{
    "ResourceRecordSets": [
        {
            "Name": "example.com.",
            "Type": "A",
            "TTL": 300,
            "ResourceRecords": [
                {
                    "Value": "➡️ 192.0.2.1"
                }
            ]
        }
    ]
}
```

---

# PART 16: CLOUDWATCH LOGS

## 16.1 LIST LOG GROUPS
```bash
aws logs describe-log-groups
```
**🔍 OUTPUT:**
```json
{
    "logGroups": [
        {
            "logGroupName": "➡️ /aws/lambda/my-function",
            "creationTime": 1234567890123
        }
    ]
}
```

## 16.2 LIST LOG STREAMS
```bash
aws logs describe-log-streams --log-group-name /aws/lambda/my-function
```
**🔍 OUTPUT:**
```json
{
    "logStreams": [
        {
            "logStreamName": "➡️ 2025/03/10/[$LATEST]123456",
            "creationTime": 1234567890123
        }
    ]
}
```

## 16.3 GET LOG EVENTS
```bash
aws logs get-log-events --log-group-name /aws/lambda/my-function --log-stream-name "2025/03/10/[$LATEST]123456"
```
**🔍 OUTPUT:**
```json
{
    "events": [
        {
            "timestamp": 1234567890123,
            "message": "➡️ Function started"
        }
    ]
}
```

---

# PART 17: ECR (ELASTIC CONTAINER REGISTRY)

## 17.1 LIST REPOSITORIES
```bash
aws ecr describe-repositories
```
**🔍 OUTPUT:**
```json
{
    "repositories": [
        {
            "repositoryName": "➡️ my-app",
            "repositoryArn": "arn:aws:ecr:us-east-1:058264439561:repository/my-app"
        }
    ]
}
```

## 17.2 LIST IMAGES
```bash
aws ecr list-images --repository-name my-app
```
**🔍 OUTPUT:**
```json
{
    "imageIds": [
        {
            "imageTag": "➡️ latest",
            "imageDigest": "sha256:1234567890abcdef"
        }
    ]
}
```

---

# PART 18: ECS (ELASTIC CONTAINER SERVICE)

## 18.1 LIST CLUSTERS
```bash
aws ecs list-clusters
```
**🔍 OUTPUT:**
```json
{
    "clusterArns": [
        "➡️ arn:aws:ecs:us-east-1:058264439561:cluster/my-cluster"
    ]
}
```

## 18.2 LIST SERVICES
```bash
aws ecs list-services --cluster my-cluster
```
**🔍 OUTPUT:**
```json
{
    "serviceArns": [
        "➡️ arn:aws:ecs:us-east-1:058264439561:service/my-cluster/my-service"
    ]
}
```

## 18.3 LIST TASKS
```bash
aws ecs list-tasks --cluster my-cluster
```
**🔍 OUTPUT:**
```json
{
    "taskArns": [
        "➡️ arn:aws:ecs:us-east-1:058264439561:task/my-cluster/1234567890abcdef"
    ]
}
```

---

# PART 19: EKS (ELASTIC KUBERNETES SERVICE)

## 19.1 LIST CLUSTERS
```bash
aws eks list-clusters
```
**🔍 OUTPUT:**
```json
{
    "clusters": [
        "➡️ my-eks-cluster"
    ]
}
```

## 19.2 DESCRIBE CLUSTER
```bash
aws eks describe-cluster --name my-eks-cluster
```
**🔍 OUTPUT:**
```json
{
    "cluster": {
        "name": "my-eks-cluster",
        "arn": "arn:aws:eks:us-east-1:058264439561:cluster/my-eks-cluster",
        "status": "ACTIVE",
        "endpoint": "➡️ https://1234567890.gr7.us-east-1.eks.amazonaws.com"
    }
}
```

---

# PART 20: SECRETS MANAGER

## 20.1 LIST SECRETS
```bash
aws secretsmanager list-secrets
```
**🔍 OUTPUT:**
```json
{
    "SecretList": [
        {
            "Name": "➡️ my-secret",
            "ARN": "arn:aws:secretsmanager:us-east-1:058264439561:secret:my-secret-123abc"
        }
    ]
}
```

## 20.2 GET SECRET VALUE
```bash
aws secretsmanager get-secret-value --secret-id my-secret
```
**🔍 OUTPUT:**
```json
{
    "Name": "my-secret",
    "SecretString": "➡️ {\"username\":\"admin\",\"password\":\"password123\"}"
}
```

## 20.3 CREATE SECRET
```bash
aws secretsmanager create-secret --name my-new-secret --secret-string "{\"apiKey\":\"abc123\"}"
```
**🔍 OUTPUT:**
```json
{
    "ARN": "arn:aws:secretsmanager:us-east-1:058264439561:secret:my-new-secret-456def",
    "Name": "my-new-secret"
}
```

## 20.4 DELETE SECRET
```bash
aws secretsmanager delete-secret --secret-id my-new-secret
```
**🔍 OUTPUT:**
```json
{
    "ARN": "arn:aws:secretsmanager:us-east-1:058264439561:secret:my-new-secret-456def",
    "Name": "my-new-secret",
    "DeletionDate": "2025-04-09T12:00:00+00:00"
}
```

---

# PART 21: GENERAL HELPFUL COMMANDS

## 21.1 AWS HELP
```bash
aws help
aws s3 help
aws ec2 describe-instances help
```

## 21.2 OUTPUT FORMATTING
```bash
# JSON output (default)
aws s3 ls --output json

# Table output (human readable)
aws s3 ls --output table

# Text output (for scripting)
aws s3 ls --output text

# Query specific fields
aws ec2 describe-instances --query 'Reservations[].Instances[].InstanceId' --output text
```

## 21.3 PAGINATION
```bash
# Limit results
aws s3api list-objects-v2 --bucket my-bucket --max-items 10

# Paginate through results
aws s3api list-objects-v2 --bucket my-bucket --page-size 100 --max-items 1000
```

## 21.4 PROFILE USAGE
```bash
# Use specific profile
aws s3 ls --profile prod

# Set default profile
export AWS_PROFILE=prod
```

## 21.5 REGION SPECIFICATION
```bash
# Specify region
aws s3 ls --region us-west-2

# Set default region
export AWS_DEFAULT_REGION=us-west-2
```

---

# 🏁 QUICK REFERENCE - MOST COMMON COMMANDS

| Service | Command | Description |
|---------|---------|-------------|
| **IAM** | `aws sts get-caller-identity` | Who am I? |
| **S3** | `aws s3 ls` | List buckets |
| **S3** | `aws s3 cp file.txt s3://bucket/` | Upload file |
| **S3** | `aws s3 cp s3://bucket/file.txt .` | Download file |
| **EC2** | `aws ec2 describe-instances` | List instances |
| **EC2** | `aws ec2 start-instances --instance-ids i-123` | Start instance |
| **EC2** | `aws ec2 stop-instances --instance-ids i-123` | Stop instance |
| **LAMBDA** | `aws lambda list-functions` | List functions |
| **DYNAMODB** | `aws dynamodb list-tables` | List tables |
| **RDS** | `aws rds describe-db-instances` | List databases |
| **SQS** | `aws sqs list-queues` | List queues |
| **SNS** | `aws sns list-topics` | List topics |
| **CLOUDWATCH** | `aws logs describe-log-groups` | List log groups |
| **KMS** | `aws kms list-keys` | List KMS keys |
| **SECRETS** | `aws secretsmanager list-secrets` | List secrets |

---

# ✅ CHECKLIST - MASTER ALL COMMANDS

- [ ] **Configuration**: `aws configure`, `aws sts get-caller-identity`
- [ ] **IAM**: Users, policies, roles, permissions
- [ ] **S3**: Buckets, upload, download, sync, presigned URLs
- [ ] **EC2**: Instances, security groups, key pairs, volumes
- [ ] **Lambda**: Functions, invoke, logs
- [ ] **DynamoDB**: Tables, items, queries
- [ ] **RDS**: Databases, snapshots
- [ ] **SQS/SNS**: Queues, topics, messages
- [ ] **CloudWatch**: Metrics, logs, alarms
- [ ] **KMS**: Keys, encryption, decryption
- [ ] **Secrets Manager**: Store, retrieve secrets

---

This is the **COMPLETE AWS CLI ZERO TO HERO GUIDE** with every command and live output examples! 🚀
