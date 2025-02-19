# AWS S3 CLI Commands

## 🗂️ List S3 Buckets
```sh
aws s3 ls
```
_Lists all S3 buckets in your AWS account._

---

## 📂 List Objects in a Bucket
```sh
aws s3 ls s3://your-bucket-name --recursive
```
_Shows all files inside a specific bucket._

---

## 🔗 Get Public URLs for S3 Objects
```sh
aws s3api list-objects --bucket your-bucket-name --query "Contents[].{URL: join('', ['https://your-bucket-name.s3.amazonaws.com/', Key])}" --output table
```
_Generates a list of public URLs for objects in a bucket._

---

## 🎟️ Generate a Pre-Signed URL (For Private Buckets)
```sh
aws s3 presign s3://your-bucket-name/path/to/file --expires-in 3600
```
_Creates a temporary URL valid for 1 hour._

---

## ⬆️ Upload a File to S3
```sh
aws s3 cp myfile.txt s3://your-bucket-name/
```
_Uploads a file from local storage to an S3 bucket._

---

## ⬇️ Download a File from S3
```sh
aws s3 cp s3://your-bucket-name/path/to/file.txt .
```
_Downloads a file from S3 to your current local directory._

---

## 🗑️ Delete a File from S3
```sh
aws s3 rm s3://your-bucket-name/path/to/file.txt
```
_Deletes a specific file from a bucket._

---

## 🔄 Sync a Local Folder with S3
```sh
aws s3 sync /local/folder s3://your-bucket-name/
```
_Syncs a local folder to S3 (uploads only new/modified files)._ 

---

## 🧹 Empty an S3 Bucket (Delete All Files)
```sh
aws s3 rm s3://your-bucket-name --recursive
```
_Deletes all objects in a bucket._

---

## 🚫 Delete an S3 Bucket
```sh
aws s3 rb s3://your-bucket-name
```
_Deletes an empty S3 bucket._

---

## ⚠️ Force Delete an S3 Bucket (Including All Files)
```sh
aws s3 rb s3://your-bucket-name --force
```
_Deletes a bucket and all its contents._

---

## 🔍 Check S3 Bucket Permissions (ACL)
```sh
aws s3api get-bucket-acl --bucket your-bucket-name
```
_Shows who has access to your S3 bucket._

---

## 🌎 Check S3 Region
```sh
aws s3api get-bucket-location --bucket your-bucket-name
```
_Displays the AWS region of a specific S3 bucket._

---

## 🛠️ Configure AWS CLI
```sh
aws configure
```
_Sets up AWS CLI with your credentials, region, and output format._

### **🔹 Modify AWS Configuration Settings**
To manually update AWS CLI settings:
```sh
aws configure set aws_access_key_id YOUR_ACCESS_KEY
aws configure set aws_secret_access_key YOUR_SECRET_KEY
aws configure set region YOUR_REGION
aws configure set output json
```
_This allows you to update specific settings without running `aws configure` again._

### **🔹 AWS CLI Configuration Persistence**
- AWS CLI stores credentials and configuration in:
  - **Credentials File:** `C:\Users\YOUR_USERNAME\.aws\credentials`
  - **Config File:** `C:\Users\YOUR_USERNAME\.aws\config`
- Your AWS CLI settings remain **even after closing and reopening** the CLI.
- To verify your configuration:
  ```sh
  aws configure list
  ```
- If your settings disappear, manually check and edit the files using:
  ```sh
  notepad $HOME\.aws\credentials
  notepad $HOME\.aws\config
  ```

---

### 🚀 Happy Cloud Computing! 🎯
