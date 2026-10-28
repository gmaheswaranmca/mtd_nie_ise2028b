# Amazon S3 — Serverless Object Storage + Static Website Hosting

> **Amazon S3 = Serverless object storage + static website hosting**

# Lab: Deploy `Portfolio Site` in S3 Bucket

## 1. Portfolio Site — Resume

Deploy a simple **Portfolio / Resume website** using an S3 bucket.

### Learn S3 Service

- Bucket creation
- File management
- S3 Versioning
- Static website hosting
- Bucket cleanup
- AWS Amplify deployment

---

# 2. Create S3 Bucket

Go to:

```text
AWS Console
    ↓
Search "S3"
    ↓
S3
    ↓
Create bucket
```

### Bucket Configuration

```text
AWS Region:
Don't change the default/current Region

Bucket type:
General purpose

Bucket namespace:
Global namespace

Bucket name:
nie-ise-3-2028-pdc-fromtrainer
```

> **Bucket name must be globally unique across Amazon S3.**

### Object Ownership

Keep the default:

```text
ACLs disabled (recommended)
```

### Block Public Access Settings

For this lab, if you are going to experiment with direct S3 public website access:

```text
Untick: Block all public access
```

Then acknowledge:

```text
I acknowledge that the current settings might result
in this bucket and the objects within becoming public.
```

### Versioning

Enable:

```text
Enable
```

Click:

```text
Create bucket
```

You should see:

```text
Successfully created bucket
```

---

# 3. Upload Portfolio Website Files

Open the bucket:

```text
S3
    ↓
Buckets
    ↓
nie-ise-3-2028-pdc-fromtrainer
```

Click:

```text
Upload
```

You can:

```text
Drag and drop files/folders
```

or use:

```text
Add files
Add folder
```

Upload your Portfolio Site files.

Example:

```text
portfolio/
├── index.html
├── style.css
├── script.js
└── images/
    └── profile.jpg
```

Click:

```text
Upload
```

After successful upload:

```text
Close
```

You can now see the uploaded files in the bucket.

---

# 4. View Uploaded Files

From the bucket's **Objects** list:

```text
Click index.html
    ↓
Open
```

You can view the content of the uploaded file.

After viewing:

```text
Close browser tab
```

> **Important:** Opening an object from the S3 console is different from viewing the complete website as a web application.

---

# 5. S3 Versioning Experiment

## Goal

Maintain multiple versions of the same file.

### Requirement

Versioning must be enabled on the bucket.

We already enabled:

```text
Bucket
    ↓
Properties
    ↓
Bucket Versioning
    ↓
Enabled
```

---

## Version 1

Upload the original:

```text
index.html
```

For example:

```html
<h1>My Portfolio - Version 1</h1>
```

---

## Version 2

Modify `index.html`:

```html
<h1>My Portfolio - Version 2</h1>
```

Go back to the bucket:

```text
Upload
    ↓
Drag and drop the new index.html
    ↓
Upload
```

The new file is uploaded successfully.

Because versioning is enabled, S3 maintains the previous version.

---

# 6. View File Versions

Go to:

```text
Bucket
    ↓
Objects
    ↓
index.html
```

Click:

```text
index.html
```

Go to the:

```text
Versions
```

tab.

You can see the different versions of the file.

### Latest Version

Select the latest version:

```text
Open
```

You can see the latest content.

### Older Version

Select the older version:

```text
Open
```

You can see the previous content.

### Learning

```text
index.html
    │
    ├── Version 1 → Old content
    └── Version 2 → Latest content
```

> **S3 Versioning helps protect against accidental overwrite or deletion by retaining previous object versions.**

---

# 7. Add Photo to Portfolio Home Page

Modify your `index.html` to display a photo.

For example:

```html
<img src="images/profile.jpg" alt="My Profile">
```

Upload:

```text
index.html
profile.jpg
```

or upload the required folder structure:

```text
images/
    └── profile.jpg
```

Check that the files are uploaded successfully.

---

## Relative Path

Your HTML may contain:

```html
<img src="images/profile.jpg">
```

Here:

```text
index.html
      ↓
images/profile.jpg
```

is a **relative path**.

The image may not display correctly when simply opening the individual HTML object from the S3 console.

Once the files are served together as a website, the browser can resolve the relative path correctly.

---

# 8. Enable S3 Static Website Hosting

Go to:

```text
Bucket
    ↓
Properties
```

Find:

```text
Static website hosting
```

Click:

```text
Edit
```

### Configure

Enable:

```text
Static website hosting
```

Select:

```text
Hosting type:
Host a static website
```

Set:

```text
Index document:
index.html
```

Click:

```text
Save changes
```

You should see:

```text
Successfully edited static website hosting
```

---

# 9. Deploy S3 Website Using AWS Amplify

For this lab, we will use **AWS Amplify to host the static website from the S3 bucket**.

Go again to:

```text
Bucket
    ↓
Properties
    ↓
Static website hosting
```

Select:

```text
View your existing Amplify apps
```

This takes you to:

```text
AWS Amplify
```

---

# 10. Create Amplify App

In Amplify:

```text
Deploy an app
```

### Step 1 — Source Code Provider

Select:

```text
Deploy without Git
```

This option is used when you want to manually deploy an application from Amazon S3 or a ZIP file.

Click:

```text
Next
```

---

# 11. Start Manual Deployment

Enter:

```text
App name:
nie-ise-3-2028-pdc-site
```

Select:

```text
Method:
Amazon S3
```

For:

```text
S3 location of objects to host
```

Click:

```text
Browse
```

Select:

```text
nie-ise-3-2028-pdc-fromtrainer
```

Click:

```text
Choose Prefix
```

Then click:

```text
Save and Deploy
```

---

# 12. Check Deployment

Amplify will start the deployment:

```text
S3 Bucket
    ↓
Amplify
    ↓
Hosting
    ↓
Deploy
```

Wait until the deployment is complete.

Then click:

```text
Visit Deployed URL
```

The website will open in the browser.

Example:

```text
https://<your-amplify-domain>.amplifyapp.com/
```

You should see your:

```text
Portfolio / Resume Website
```

---

# 13. Final Architecture

```text
                 Internet User
                       |
                       v
                 AWS Amplify
                       |
                       v
                  S3 Bucket
                       |
             +---------+---------+
             |         |         |
             v         v         v
          HTML       CSS        JS
             |
             v
       Portfolio Site
```

### Important Learning

```text
S3
 ↓
Stores website files

S3 Versioning
 ↓
Maintains previous versions

S3 Static Website Hosting
 ↓
Allows static website hosting

Amplify
 ↓
Provides managed hosting/deployment
for the static website
```

---

# 14. Cleanup

## Delete Amplify App

Go to:

```text
AWS Amplify
    ↓
All Apps
    ↓
nie-ise-3-2028-pdc-site
```

Then:

```text
App Settings
    ↓
General Settings
    ↓
Delete App
```

Confirm deletion.

---

## Empty S3 Bucket

Go to:

```text
S3
    ↓
Buckets
    ↓
nie-ise-3-2028-pdc-fromtrainer
```

Select:

```text
Empty
```

Delete all objects and versions as prompted.

---

## Delete S3 Bucket

After the bucket is empty:

```text
S3
    ↓
Buckets
    ↓
Select bucket
    ↓
Delete
```

Enter the bucket name when AWS asks for confirmation.

---

# 15. Lab Flow — Easy Memory

```text
Create S3 Bucket
       ↓
Upload Website
       ↓
Enable Versioning
       ↓
Modify index.html
       ↓
Upload Again
       ↓
Observe Multiple Versions
       ↓
Enable Static Website Hosting
       ↓
Open Amplify
       ↓
Deploy Without Git
       ↓
Select Amazon S3
       ↓
Select S3 Bucket
       ↓
Save and Deploy
       ↓
Visit Deployed URL
       ↓
Test Portfolio Website
       ↓
Cleanup
```

### One-Line Memory

> **S3 stores → Versioning preserves → Static Website Hosting serves → Amplify deploys and hosts the Portfolio Site.**