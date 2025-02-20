# AWS S3 CLI Commands

## 🗂️ List S3 Buckets
```sh
aws s3 ls
```
_Lists all S3 buckets in your AWS account._

---

## 🛠️ Create a New S3 Bucket
```sh
aws s3api create-bucket --bucket your-bucket-name --region your-region --create-bucket-configuration LocationConstraint=your-region
```
_Creates a new S3 bucket in the specified region._

Example:
```sh
aws s3api create-bucket --bucket my-new-bucket --region us-east-1 --create-bucket-configuration LocationConstraint=us-east-1
```

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

## 🔄 Managing S3 Versioning and Permissions

### **1️⃣ Understanding S3 Versioning Behavior**
- **Latest version inherits permissions by default**, while **older versions retain their original permissions**.
- When a **new version** of an object is uploaded, **previous versions do not automatically update their permissions**.
- If permissions are changed at the bucket level, they **do not retroactively apply** to older versions of an object.

---

### **2️⃣ List All Versions of an Object**
```sh
aws s3api list-object-versions --bucket your-bucket-name --prefix your-object-key
```
_Lists all versions of a specific object._

---

### **3️⃣ Download a Specific Version**
```sh
aws s3api get-object --bucket your-bucket-name --key your-object-key --version-id YOUR_VERSION_ID your-local-file
```
_Downloads an older version of an object._

---

### **4️⃣ Restore an Older Version as the Latest**
```sh
aws s3 cp s3://your-bucket-name/your-object-key s3://your-bucket-name/your-object-key --version-id YOUR_VERSION_ID
```
_Makes an older version the latest by copying it back._

---

### **5️⃣ Apply Public Read to an Older Version**
```sh
aws s3api put-object-acl --bucket your-bucket-name --key your-object-key --version-id YOUR_VERSION_ID --acl public-read
```
_Grants public read access to a specific older version._

---

### **6️⃣ Apply Public Read to All Versions (Loop)**
```sh
for version in $(aws s3api list-object-versions --bucket your-bucket-name --prefix your-object-key --query "Versions[].VersionId" --output text); do
    aws s3api put-object-acl --bucket your-bucket-name --key your-object-key --version-id $version --acl public-read;
done
```
_Grants public read access to all versions of an object._

---

## 🚨 Fix Access Denied When Applying a Bucket Policy

### **1️⃣ Allow Public Policies by Disabling Block Public Access**
```sh
aws s3api put-public-access-block --bucket your-bucket-name --public-access-block-configuration "BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false"
```
_This allows public bucket policies to be applied._

---

### **2️⃣ Apply the Bucket Policy Again**
```sh
aws s3api put-bucket-policy --bucket your-bucket-name --policy "file://C:\Users\YOUR_USERNAME\Desktop\AWS_Stuff\bucket-policy.json"
```
_This applies the previously defined policy to the bucket._

---

### **3️⃣ Verify the Bucket Policy**
```sh
aws s3api get-bucket-policy --bucket your-bucket-name
```
_This confirms that the policy was applied successfully._

---

## 🔹 Generate a Policy Using AWS Policy Generator
You can use AWS’s **Policy Generator** to create custom policies:

🔗 **AWS Policy Generator:** [https://awspolicygen.s3.amazonaws.com/policygen.html](https://awspolicygen.s3.amazonaws.com/policygen.html)

To generate a policy:
1. Select **Policy Type** (S3 Bucket Policy).
2. Choose **Effect** (Allow or Deny).
3. Specify the **Principal** (who gets access).
4. Choose **Actions** (e.g., `s3:GetObject`, `s3:PutObject`).
5. Enter the **Resource ARN** (`arn:aws:s3:::your-bucket-name/*`).
6. Click **Generate Policy** and copy the JSON output.
7. Apply it to your bucket using:
   ```sh
   aws s3api put-bucket-policy --bucket your-bucket-name --policy file://generated-policy.json
   ```

---

### 🚀 Happy Cloud Computing! 🎯
