---
title: "How to Host a Website Using AWS Route 53 and S3 with a GoDaddy Domain – A Beginner-Friendly Guide"
seoTitle: "How to Connect a GoDaddy Domain to AWS Route 53 and Host a Static Webs"
seoDescription: "Learn how to connect your GoDaddy domain to AWS Route 53 and host a static website on Amazon S3. This step-by-step beginner-friendly guide covers domain set"
datePublished: Sun Feb 02 2025 17:12:06 GMT+0000 (Coordinated Universal Time)
cuid: cm6nvqeve000009l55e3oclzx
slug: how-to-host-a-website-using-aws-route-53-and-s3-with-a-godaddy-domain-a-beginner-friendly-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1738516187681/76e405e5-8c71-48b7-a0c6-7743ac527689.webp
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1738516283764/de67ef0b-41c8-4bb0-8c33-a8eb3c69351c.webp
tags: godaddy, aws, deployment, devops, s3, route53

---

So, you have a **domain** (like `mycoolwebsite.com`), and you want to **connect it to a website** hosted on AWS? Sounds complicated? **Don't worry!** 😃

In this guide, I will explain **everything** step by step in a way that even a complete beginner can follow.

We'll do **three things**:

1. **Buy a domain from GoDaddy** (if you don’t have one).
    
2. **Host a static website on AWS S3** (Static means no database, just HTML, CSS, and JavaScript).
    
3. **Connect your domain to AWS using Route 53** (so your site loads when people type your domain name).
    

> 💡 **Think of Route 53 as a "phonebook"** that tells browsers where to find your website files stored in S3.

---

## **🌟 What You’ll Need**

✔ A **GoDaddy** account (to manage your domain).  
✔ An **AWS account** (for hosting your website).  
✔ Some basic website files (`index.html`, `style.css`, etc.).

If you don’t have a website yet, don’t worry! I will give you a simple HTML file later. 😃

---

## **Step 1: Buy a Domain from GoDaddy (Skip If You Already Have One)**

1️⃣ **Go to** [GoDaddy.com](https://www.godaddy.com/).  
2️⃣ Type the domain name you want in the search bar (e.g., `mycoolwebsite.com`).  
3️⃣ If the domain is available, click **Add to Cart**.  
4️⃣ Click **Continue to Cart** and follow the checkout process.

💡 **Tip:** You don’t need extras like “Privacy Protection” unless you want them.

---

## **Step 2: Create an S3 Bucket for Your Website on AWS**

### **1️⃣ Open S3**

1. Go to **AWS Console** → Click on **Services** (top-left).
    
2. Search for **S3** and click on it.
    

### **2️⃣ Create a New Bucket**

1. Click **“Create bucket”** (big orange button).
    
2. **Bucket name** → Enter your domain name (e.g., `mycoolwebsite.com`).
    
3. **AWS Region** → Choose the closest region to your visitors.
    

💡 **Important:** Your **bucket name must be the same as your domain name** (`mycoolwebsite.com`).

4. Scroll down to the **"Block Public Access settings"** section:
    
    * Uncheck **“Block all public access”** (We want the public to see our website).
        
    * A warning will appear—just check the box **“I acknowledge that…”**
        
5. Click **“Create bucket”**.
    

---

## **Step 3: Upload Website Files to S3**

1. Open your **new S3 bucket**.
    
2. Click **“Upload”** (top-right).
    
3. Drag and drop your **HTML, CSS, and JS files** (or click “Add files” to select them).
    
4. Click **Upload**.
    

💡 **If you don’t have a website yet, create a simple** `index.html` file with this content:

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Cool Website</title>
</head>
<body>
    <h1>Welcome to My Cool Website!</h1>
</body>
</html>
```

---

## **Step 4: Enable S3 Static Website Hosting**

1. Open your S3 bucket.
    
2. Click the **"Properties"** tab.
    
3. Scroll down to **"Static website hosting"**.
    
4. Click **"Edit"** → Select **"Enable"**.
    
5. Set:
    
    * **Index document** → `index.html`
        
    * **Error document** → `error.html` (optional)
        
6. Click **Save changes**.
    
7. **Copy the "Bucket website endpoint"** (It looks like `http://mycoolwebsite.s3-website-us-east-1.amazonaws.com`).
    

---

## **Step 5: Allow Public Access to Your Website**

By default, S3 blocks access to your files. Let's fix that:

1. Go to your **S3 bucket** → Click **"Permissions"**.
    
2. Scroll to **"Bucket policy"** → Click **"Edit"**.
    
3. Paste this policy (replace `mycoolwebsite.com` with your actual bucket name):
    

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::mycoolwebsite.com/*"
        }
    ]
}
```

4. Click **Save changes**.
    

Now, when you open the **Bucket Website Endpoint**, your site should be visible! 🎉

---

## **Step 6: Set Up Route 53 for Your Domain**

Now that we have our **website hosted on S3** and our **domain registered in GoDaddy**, we need to **set up Route 53** to manage our domain’s DNS.

### **🔹 What is Route 53 and Why Do We Need It?**

* **Route 53 is AWS’s Domain Name System (DNS) service**.
    
* It helps connect your **domain name** (e.g., [`mycoolwebsite.com`](http://mycoolwebsite.com)) to the actual website files stored in **S3**.
    
* Right now, **GoDaddy** manages your domain’s DNS. We need to **transfer DNS management to Route 53** so we can properly connect it to AWS.
    

---

## **🔹 Steps to Set Up Route 53 for Your Domain**

### **1️⃣ Open Route 53 in AWS**

1. Open a **web brow**[**ser** and go](https://aws.amazon.com/) to [AWS Console](https://aws.amazon.com/).
    
2. **Sign in** to your AWS account.
    
3. In the **AWS search bar**, type **Route 53** and click on it.
    

---

### **2️⃣ Create a Hos**[**ted Zone**](https://aws.amazon.com/)

1. Click on **"Hosted Zones"** (left side[bar).](https://aws.amazon.com/)
    
2. [Cl](https://aws.amazon.com/)ick **"Create Hosted Zone"** (big orange button)[.](https://aws.amazon.com/)
    

---

### [**3️⃣ E**](https://aws.amazon.com/)**nter Your Domain Name**

1. In the **"Domain name"** field, enter your domain name exactly as it appe[ars in GoDa](https://aws.amazon.com/)ddy.
    
    * Example: [`mycoolwebsite.com`](http://mycoolwebsite.com) (no `www.`).
        
2. Under **"Type"**, select **"Public Hosted Zone"** (because this is a public websi[te).](https://aws.amazon.com/)
    
3. [Cli](https://aws.amazon.com/)ck **"Create hosted zone"**.
    

---

### **4️⃣ G**[**et Your AWS**](https://aws.amazon.com/) **Name Servers**

Af[ter creatin](https://aws.amazon.com/)g the hosted zone, AWS will automa[tically gen](https://aws.amazon.com/)erate **four Name Serve**[**r (NS) reco**](https://aws.amazon.com/)**rds**. These look like this:

```javascript
ns-1234.awsdns-56.com  
ns-5678.awsdns-78.net  
ns-9101.awsdns-90.org  
ns-1213.awsdns-12.co.uk  
```

📌 **Copy all four name servers** – we [will use t](https://aws.amazon.com/)hem in GoDaddy in t[he next ste](https://aws.amazon.com/)p.

## **Step 7: Connect Your GoDaddy Domain to AWS Route 53 (Beginner-Friendly Gu**[**ide)**](https://aws.amazon.com/)

[Now t](https://aws.amazon.com/)hat we have created a **Hosted Zon**[**e** in Route](https://aws.amazon.com/) 53 and got our **Name Servers**, it's time to **connect your GoDaddy domain** [**to AWS**.](https://aws.amazon.com/)

[Th](https://aws.amazon.com/)is step is important [because it](https://aws.amazon.com/) tells the internet t[hat **your do**](https://aws.amazon.com/)**main name should point to AWS**, where your website is hosted.

Let’s break it down **slowly and in detail** so even a complete beginner can follow! 🚀

---

## **🔹 What Are Name Servers? (Simple Explanation)**

Right now, your dom[ain is **regi**](https://aws.amazon.com/)**stered with GoDaddy**, but **GoDaddy doesn’t know** where your website is stored.

**Name Servers** are like the **GPS coordinates** of your website. They tell GoDaddy, **“Hey, this website is actually hosted on AWS!”**

We will replace **GoDaddy’s default name servers** with **AWS name servers** from Route 53.

---

## **🔹 Steps to Change Name Servers in GoDaddy**

### **1️⃣ Log in to Your GoDaddy Account**

1. Open a browser (Chrome, Safari, Edge, etc.).
    
2. Go to [**GoDaddy.com**](https://www.godaddy.com/).
    
3. Click **Sign In** (top right corner).
    
4. Enter your **GoDaddy email/username** and **password** → Click **Sign In**.
    

---

### **2️⃣ Open Your Domain Settings**

1. After logging in, you’ll see your **GoDaddy Dashboard**.
    
2. Click **"My Products"** (top menu).
    
3. Scroll down to the **Domains** section.
    
4. Find the domain name you want to use (e.g., `mycoolwebsite.com`).
    
5. Click the **"Manage"** button next to your domain.
    

---

### **3️⃣ Find the Nameservers Section**

1. After clicking "Manage," you will see your domain’s settings page.
    
2. Scroll down until you find the **"Nameservers"** section.
    
3. Here, you will see something like this:
    
    ```javascript
    ns01.domaincontrol.com  
    ns02.domaincontrol.com  
    ```
    
    These are **GoDaddy’s default name servers**. We need to **replace them with AWS name servers**.
    

---

### **4️⃣ Change the Nameservers**

1. In the **Nameservers** section, click the **"Change"** button.
    
2. A new window will pop up asking how you want to change your name servers.
    

---

### **5️⃣ Select "Enter My Own Nameservers"**

1. In the pop-up window, select **"Enter my own nameservers (advanced)"**.
    
2. Now, GoDaddy will allow you to enter your **own custom name servers** (which we got from Route 53).
    

---

### **6️⃣ Copy-Paste AWS Name Servers from Route 53**

1. Open a **new tab** in your browser and go to the **AWS Console**.
    
2. Click on **Services** → Search for **Route 53** and open it.
    
3. Click on **"Hosted Zones"**.
    
4. Click on your **domain name** (`mycoolwebsite.com`).
    
5. Look for the **NS (Name Server) record** – you will see **four name servers** like this:
    
    ```javascript
    ns-1234.awsdns-56.com  
    ns-5678.awsdns-78.net  
    ns-9101.awsdns-90.org  
    ns-1213.awsdns-12.co.uk  
    ```
    
6. Copy **all four** name servers **one by one** and paste them into GoDaddy.
    
    * Go back to **GoDaddy’s Nameserver settings**.
        
    * You will see **four empty boxes** where you can enter the name servers.
        
    * Paste **each name server** into a separate box.
        
    
    ✅ Example (in GoDaddy):
    
    ```javascript
    ns-1234.awsdns-56.com  
    ns-5678.awsdns-78.net  
    ns-9101.awsdns-90.org  
    ns-1213.awsdns-12.co.uk  
    ```
    

---

### **7️⃣ Save the Changes**

1. After pasting the **four name servers**, double-check to make sure they are **correct**.
    
2. Click **"Save"** or **"Confirm"**.
    
3. GoDaddy may ask for **confirmation** – if so, click **Yes** or **Confirm**.
    

---

### **8️⃣ Wait for the Changes to Take Effect**

* DNS changes **don’t happen instantly**.
    
* It can take anywhere from **30 minutes to 48 hours** for the internet to update the new name servers.
    
* In most cases, it happens within **a few hours**.
    

## **Step 8: Create an A Record in Route 53 (Connecting Your Domain to S3 Website)**

Now that we have set up our domain in **GoDaddy** and pointed it to **AWS Route 53**, we need to create an **A Record** in Route 53.

**What is an A Record? 🤔**  
An **A Record (Address Record)** tells the internet **where to find your website** when someone types your domain (`mycoolwebsite.com`).

Since we are hosting our website on **Amazon S3**, we need to **connect our domain to our S3 bucket’s website endpoint** using an **A Record with Alias**.

---

## **🔹 Steps to Create an A Record in Route 53**

### **1️⃣ Open Route 53 in AWS**

1. Open a **web browser** and go to [AWS Console](https://aws.amazon.com/).
    
2. Sign in to your AWS account.
    
3. In the AWS **search bar**, type **Route 53** and click on it.
    
4. On the left menu, click **"Hosted Zones"**.
    
5. Find and **click on your domain name** (`mycoolwebsite.com`).
    

---

### **2️⃣ Create a New A Record**

1. Inside your **Hosted Zone**, click **"Create Record"** (big orange button).
    
2. You will now see a form to enter record details.
    

---

### **3️⃣ Configure the A Record**

#### **👀 What You’ll See on the Screen & What to Enter**

| **Field Name** | **What to Do** |
| --- | --- |
| **Record Name** | Leave it **blank** (for the root domain like `mycoolwebsite.com`). |
| **Record Type** | Select **A - IPv4 address**. |
| **Alias** | Select **"Yes"**. |
| **Alias Target** | Click inside the box and choose your **S3 Bucket Website Endpoint** from the list. |
| **Routing Policy** | Leave it as **Simple routing** (default). |
| **Evaluate Target Health** | Leave it **unchecked**. |

---

### **4️⃣ Save the A Record**

1. Double-check that your **Alias Target** is pointing to your S3 bucket.
    
2. Click **"Create Record"** (big orange button at the bottom).
    
3. Your **A Record is now created**! 🎉
    

---

## **Step 9: Test Your Website (Does It Work?)**

After setting up everything, it’s time to check if your website is **live and working**.

### **1️⃣ Open Your Website in a Browser**

* Open **Google Chrome, Safari, Edge, or Firefox**.
    
* In the **address bar**, type **your domain name** (`mycoolwebsite.com`).
    
* Press **Enter**.
    

---

### **2️⃣ What Should Happen?**

✅ **If everything is set up correctly** → Your **website will load!** 🎉  
❌ **If it doesn’t load** → Don’t panic! Sometimes DNS changes take time.

---

## **🔹 What If My Website Doesn't Work?**

**Here are some common reasons & fixes:**

1️⃣ **DNS is still updating**

* **Wait for 30 minutes to a few hours** (DNS propagation can take time).
    
* Check your **domain’s DNS records** using [https://who.is/](https://who.is/) to see if the Name Servers are updated.
    

2️⃣ **S3 Bucket is Not Public**

* Go to **S3** → Open your **bucket** → Check if **your files are public**.
    
* If needed, follow **Step 5** to update the **Bucket Policy**.
    

3️⃣ **Wrong A Record Configuration in Route 53**

* Go back to **Route 53 Hosted Zone** and make sure:
    
    * The **A Record exists**.
        
    * The **Alias Target** is correctly pointing to your **S3 Website Endpoint**.
        

# **Conclusion**

Congratulations! You just: ✅ Bought a domain (GoDaddy)  
✅ Hosted a static website (AWS S3)  
✅ Connected your domain (Route 53)

Now, your website is **live** on the internet! 🚀

👉 **Next Steps?**

* Add **HTTPS (SSL)** using **AWS CloudFront**.
    
* Improve site **speed** with a **CDN**.
    

Want help with SSL? Let me know! 😊