# Working with EC2 as Static Web Site Hosting

## 1. Basic Concepts

| Term | Simple Meaning |
|---|---|
| **EC2** | A virtual server in the AWS Cloud |
| **Security Group** | A virtual firewall for the EC2 server |
| **SSH** | Secure Shell — securely connects to a remote Linux server |
| **HTTP** | Protocol used to access web pages |
| **Apache HTTP Server** | Web server software that serves HTML/CSS/JS files |
| **Public IP** | Internet-accessible IP address of the EC2 server |

### Goal

We will create this setup:

```text
Internet User
      |
      | HTTP : 80
      v
+-------------------------+
| EC2 Instance            |
| resume_site_server      |
|                         |
| Amazon Linux            |
| Apache (httpd)          |
|                         |
| /var/www/html/index.html|
+-------------------------+
```

---

# 2. Resources We Will Create

### Key Pair

```text
Name       : resume_site_key
Type       : RSA
File       : .pem
```

### Security Group

```text
Name       : resume_site_sg
Description: resume_site_sg
```

**Inbound Rules**

| Type | Port | Source |
|---|---:|---|
| SSH | 22 | Anywhere IPv4 `0.0.0.0/0` |
| HTTP | 80 | Anywhere IPv4 `0.0.0.0/0` |

**Outbound Rules**

```text
All Traffic → Anywhere IPv4
```

### EC2 Instance

```text
Name         : resume_site_server
AMI          : Amazon Linux
Instance     : t3.micro
Key Pair     : resume_site_key
Security     : resume_site_sg
Storage      : 8 GiB gp3
```

---

# 3. Create Key Pair

### Step 1: Open EC2

```text
AWS Console
   ↓
Search "EC2"
   ↓
EC2
```

### Step 2: Open Key Pairs

```text
EC2
 → Left Menu
 → Network & Security
 → Key Pairs
```

### Step 3: Create Key Pair

Click:

```text
Create key pair
```

Enter:

```text
Name          : resume_site_key
Key pair type : RSA
Private key   : .pem
```

Click:

```text
Create key pair
```

The `.pem` file will be downloaded.

> **Important:** Keep the `.pem` file safe. It is used for SSH access.

---

# 4. Create Security Group

Go to:

```text
EC2
 → Network & Security
 → Security Groups
```

Click:

```text
Create security group
```

### Basic Details

```text
Security group name : resume_site_sg
Description         : resume_site_sg
```

### Inbound Rules

Add:

```text
SSH
Port : 22
Source : Anywhere IPv4
```

Add:

```text
HTTP
Port : 80
Source : Anywhere IPv4
```

So:

```text
SSH  → 22 → 0.0.0.0/0
HTTP → 80 → 0.0.0.0/0
```

### Outbound Rules

Keep the default:

```text
All Traffic → Anywhere IPv4
```

Click:

```text
Create security group
```

Verify the security group in the list.

---

# 5. Launch EC2 Instance

Go to:

```text
EC2
 → Instances
 → Instances
```

Click:

```text
Launch instances
```

### Instance Configuration

#### Name

```text
resume_site_server
```

#### AMI

Select:

```text
Amazon Linux
```

#### Instance Type

```text
t3.micro
```

#### Key Pair

Select:

```text
resume_site_key
```

#### Network Settings

Use the default VPC.

Select:

```text
resume_site_sg
```

#### Storage

Keep the default:

```text
8 GiB gp3
```

Click:

```text
Launch instance
```

You should see a message indicating that the instance launch was initiated.

---

# 6. Check EC2 Instance

Go to:

```text
EC2
 → Instances
 → Instances
```

Select:

```text
resume_site_server
```

Check:

```text
Instance state : Running
```

Initially:

```text
Status check : Initializing
```

Wait until:

```text
3/3 checks passed
```

---

# 7. Connect Using EC2 Instance Connect

Select the instance.

Click:

```text
Connect
```

Select:

```text
EC2 Instance Connect
```

Click:

```text
Connect
```

A browser-based Linux terminal will open.

Try:

```bash
pwd
```

Shows the present working directory.

```bash
whoami
```

Shows the current user.

```bash
ls
```

Lists files/directories.

```bash
ls /
```

Lists the contents of the Linux root directory.

Finally:

```bash
exit
```

---

# 8. Connect Using SSH

Select:

```text
EC2 Instance
 → Connect
 → SSH Client
```

Under:

```text
Connect to your instance using its Public DNS
```

AWS provides an SSH command similar to:

```bash
ssh -i "resume_site_key.pem" ec2-user@ec2-3-107-39-232.ap-southeast-2.compute.amazonaws.com
```

### Go to the `.pem` File Directory

For example, if the file is in Downloads:

```bash
cd ~/Downloads
```

Then run the SSH command provided by AWS:

```bash
ssh -i "resume_site_key.pem" ec2-user@<public-dns>
```

The first time, you may see a fingerprint confirmation:

```text
Are you sure you want to continue connecting?
```

Enter:

```text
yes
```

You are now connected to the EC2 Linux server.

Test:

```bash
pwd
```

```bash
whoami
```

---

# 9. Install Apache Web Server

Now work inside the EC2 server.

### Step 1: Update packages

```bash
sudo dnf update -y
```

### Step 2: Install Apache

```bash
sudo dnf install httpd -y
```

Apache is installed.

---

# 10. Start Apache

```bash
sudo systemctl start httpd
```

Enable Apache so that it starts automatically when the server boots:

```bash
sudo systemctl enable httpd
```

Check its status:

```bash
sudo systemctl status httpd
```

You should see something similar to:

```text
Active: active (running)
```

Press:

```text
q
```

to leave the status screen.

---

# 11. Deploy a Static Web Page

Apache's default web directory is:

```text
/var/www/html
```

Move into it:

```bash
cd /var/www/html
```

Create the page:

```bash
sudo nano index.html
```

Enter:

```html
<h1>I am from NIE</h1>
```

### Save the file

Inside `nano`:

```text
Ctrl + O
```

Press:

```text
Enter
```

Then:

```text
Ctrl + X
```

---

# 12. Check the Web Page

Run:

```bash
cat index.html
```

You should see:

```html
<h1>I am from NIE</h1>
```

At this point:

```text
index.html
      ↓
/var/www/html
      ↓
Apache httpd
      ↓
HTTP : 80
      ↓
Internet
```

---

# 13. Open the Website

Go to the EC2 instance details page.

Find:

```text
Public IPv4 address
```

For example:

```text
13.xx.xx.xx
```

Open the browser and enter:

```text
http://<your-public-ip>
```

**Use `http`, not `https`.**

For example:

```text
http://13.xx.xx.xx
```

You should see:

# I am from NIE

🎉 Your EC2 instance is now working as a **web server hosting a static website**.

---

# 14. What Happened?

The complete flow is:

```text
                         Internet
                            |
                            | HTTP : 80
                            v
                    +---------------+
                    | Security      |
                    | Group         |
                    | Port 80       |
                    +-------+-------+
                            |
                            v
                    +---------------+
                    | EC2           |
                    | Amazon Linux   |
                    +-------+-------+
                            |
                            v
                    +---------------+
                    | Apache httpd   |
                    +-------+-------+
                            |
                            v
                    +---------------+
                    | /var/www/html |
                    |               |
                    | index.html     |
                    +---------------+
```

### Important distinction

EC2 itself is **not** the web server.

```text
EC2       = Virtual Server
Apache    = Web Server
index.html = Website
Security Group = Firewall
SSH       = Remote administration connection
```

---

# 15. Cleanup

When the experiment is finished:

### 1. Terminate EC2

```text
EC2
 → Instances
 → Select resume_site_server
 → Instance state
 → Terminate instance
```

### 2. Delete Security Group

```text
EC2
 → Security Groups
 → Select resume_site_sg
 → Delete
```

### 3. Delete Key Pair

```text
EC2
 → Network & Security
 → Key Pairs
 → Select resume_site_key
 → Delete
```

### 4. Delete `.pem` File

Delete the downloaded:

```text
resume_site_key.pem
```

from your local `Downloads` folder **only after you are sure you no longer need it**.

---

# Final Learning Summary

```text
1. Create Key Pair
       ↓
2. Create Security Group
       ↓
3. Launch EC2
       ↓
4. Connect to Linux using EC2 Instance Connect
       ↓
5. Connect using SSH
       ↓
6. Install Apache
       ↓
7. Start + Enable Apache
       ↓
8. Create index.html
       ↓
9. Access using http://<Public-IP>
       ↓
10. Cleanup
```

**Memory shortcut:**

> **EC2 = Computer**  
> **Security Group = Firewall**  
> **SSH = Remote Login**  
> **Apache = Web Server**  
> **`/var/www/html` = Website Folder**  
> **Port 80 = HTTP Web Traffic**