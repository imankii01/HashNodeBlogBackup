---
title: "🚀 Day-11: Next Steps After Hosting a Website on AWS S3 & Route 53 🚀"
seoDescription: "Next Steps After Hosting a Website on AWS S3 & Route 53 "
datePublished: Mon Feb 03 2025 17:18:56 GMT+0000 (Coordinated Universal Time)
cuid: cm6pbf1gv000109l1cad4fqvm
slug: day-11-next-steps-after-hosting-a-website-on-aws-s3-route-53
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1738603073507/59b01c8c-380b-4c9b-afe2-91092d3f6607.webp
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1738603120054/5e828e07-a324-4068-867e-bc47ce32993f.webp
tags: aws, cloudfront-distribution

---

In **Day-10**, we successfully hosted a static website on **AWS S3** and configured **Route 53** with a **GoDaddy domain**. But launching a website is just the beginning! Now, let’s take it a step further by improving security and performance.

In this guide, we will:  
✅ **Secure the website with HTTPS (SSL) using AWS CloudFront**  
✅ **Improve website speed and global availability with a CDN**

---

## **📌 Table of Contents**

1️⃣ **Introduction**  
2️⃣ **Why Add HTTPS & CDN?**  
3️⃣ **Step 1: Set Up AWS CloudFront for SSL (HTTPS)**  
4️⃣ **Step 2: Configure S3 for CloudFront Integration**  
5️⃣ **Step 3: Update Route 53 for CloudFront**  
6️⃣ **Step 4: Enable Caching & Improve Performance**  
7️⃣ **Testing & Finalizing the Setup**  
8️⃣ **Conclusion**

---

## **1️⃣ Introduction**

So, you've successfully hosted your website on AWS S3 and connected it to your domain using Route 53. **Great job!** 🎉 But hold on—there’s still more to do!

Right now, your website is **not secure (no HTTPS)** and might **load slowly** for visitors who are far from your AWS region. This is because:

1️⃣ **No SSL (HTTPS) → Security Issues**

* If a website doesn’t have HTTPS, browsers (like Chrome) show a **“Not Secure”** warning.
    
* Without SSL, **data sent between your website and users is not encrypted**, making it vulnerable to hackers.
    

2️⃣ **No CDN (Content Delivery Network) → Slow Load Times**

* If your AWS S3 bucket is hosted in one region (e.g., US East), users from other parts of the world (e.g., India, Europe) **will experience slow loading times**.
    
* Without a CDN, every user must load your website **directly from your AWS S3 region**, which takes longer the farther they are.
    

## **2️⃣ Why Add HTTPS & CDN?**

Imagine you’re opening a **bank website** to check your balance. Would you want your connection to be unencrypted and at risk of being hacked? **Of course not!** That’s why **every professional website today uses HTTPS** to protect user data.

Similarly, if you’re watching a YouTube video, it **loads instantly** because YouTube uses a **CDN** to store copies of its content in different locations worldwide. This ensures **fast delivery no matter where you are**.

By setting up **AWS CloudFront (a CDN)** and **SSL (HTTPS)**, we can:  
✔ **Make the website secure** 🛡 (No browser warnings!)  
✔ **Load pages faster** ⚡ (Better experience for users worldwide!)  
✔ **Improve SEO rankings** 📈 (Google prefers HTTPS websites!)

🔹 In this guide, I’ll walk you through the exact steps to:  
✅ **Enable SSL (HTTPS) using AWS CloudFront** (so your site is secure).  
✅ **Speed up loading time with a CDN** (so visitors from any country can access it faster).

This might sound technical, but don’t worry—**I’ll explain everything in a super simple way, step by step.** By the end, your website will be **secure, fast, and optimized for global users!** 🚀

---

## **2️⃣ Why Add HTTPS & CDN?7️⃣ Testing & Finalizing the Setup**

🛠️ **Test Your Website:**

* Open your domain in a browser and check if HTTPS is enabled.
    
* Run a **CDN speed test** (e.g., GTMetrix, Pingdom) to confirm faster loading.
    

📌 **Fix Common Issues:**

* If SSL isn’t working, check **ACM Certificate validation**.
    
* If the website isn’t loading, check **CloudFront distribution settings**.
    

## **3️⃣ Step 1: Set Up AWS CloudFront for SSL (HTTPS)**

### **🔹 What is AWS CloudFront?**

AWS **CloudFront** is a **Content Delivery Network (CDN)** that helps distribute your website globally. Instead of serving your website directly from an **S3 bucket (single AWS region)**, CloudFront **caches and delivers content from multiple edge locations worldwide**, making it:  
✅ **Faster** – Loads website content from the nearest AWS location.  
✅ **More Secure** – Supports **HTTPS (SSL)** for data encryption.  
✅ **Cost-Effective** – Reduces repeated requests to S3, saving AWS costs.

Now, let’s set up **CloudFront** step by step:

---

### **🛠️ Step 1: Open CloudFront in AWS Console**

1️⃣ **Log in to AWS Console** → [https://aws.amazon.com/console/](https://aws.amazon.com/console/)  
2️⃣ In the **Search Bar**, type `CloudFront` and click on **CloudFront** from the search results.  
3️⃣ Click on **"Create Distribution"** (blue button).

---

### **🛠️ Step 2: Configure Origin Settings**

The **Origin** is the source of your website content. Since we hosted our website in an **S3 bucket**, we will set it as the origin.

#### **🔹 Origin Domain**

* Click on the **“Origin Domain Name”** box.
    
* Select your **S3 Bucket** from the dropdown list.
    
* **DO NOT** select the option that ends with `.s3.amazonaws.com`. Instead, pick the **full bucket name** (without `.s3` in the URL).
    

✅ Example: **mywebsite-bucket.s3.us-east-1.amazonaws.com**

#### **🔹 Origin Path (Optional)**

* Leave this **empty** unless you are hosting your website inside a subfolder in S3.
    

#### **🔹 Origin Access**

* Select **“Origin access control settings (recommended)”**
    
* Click on **“Create Control Setting”**
    
* Give it a name (e.g., **CloudFront-S3-Access**)
    
* Click **Create** and then select it in the dropdown.
    

✅ This ensures **only CloudFront can access your S3 bucket**, making it more secure.

#### **🔹 Protocol**

* Choose **"HTTPS Only"** (because we want secure access).
    

---

### **🛠️ Step 3: Configure Default Cache Settings**

CloudFront caches website content **to speed up loading times**. Let’s configure it properly:

#### **🔹 Cache Policy**

* Click on **“Cache policy”** dropdown.
    
* Select **“CachingOptimized”** (recommended for websites).
    

#### **🔹 Compress Objects Automatically**

* Select **"Yes"** (this enables gzip & Brotli compression for faster loading).
    

✅ This reduces file sizes, making your website load quicker for users!

---

### **🛠️ Step 4: Enable HTTPS (SSL) for Secure Connection**

Now, let’s **enable HTTPS** to make the website secure:

#### **🔹 Viewer Protocol Policy**

* Select **"Redirect HTTP to HTTPS"** (forces all visitors to use HTTPS).
    

✅ This ensures **no one can access your site over an insecure connection (HTTP).**

#### **🔹 Allowed HTTP Methods**

* Choose **"GET, HEAD, OPTIONS"** (recommended for static websites).
    

✅ This limits requests to only reading content, preventing unnecessary updates.

---

### **🛠️ Step 5: Configure SSL Certificate for HTTPS**

CloudFront allows **free SSL certificates** via **AWS Certificate Manager (ACM)**.

#### **🔹 Custom SSL Certificate**

* Choose **"Custom SSL Certificate"**
    
* Click on **"Request or Import a Certificate with ACM"**
    

🔹 **How to Request an SSL Certificate?**  
1️⃣ This will take you to **AWS Certificate Manager (ACM)**.  
2️⃣ Click **"Request a Certificate"**.  
3️⃣ Select **“Public Certificate”** (this is required for websites).  
4️⃣ In the **"Domain Name"** field, enter your website domain (e.g., `example.com`).  
5️⃣ Click **“Next”** and follow the steps to verify your domain via email.

✅ Once the certificate is issued, return to CloudFront and **select it** from the dropdown.

---

### **🛠️ Step 6: Configure Custom Error Pages (Optional)**

If you want a custom **404 (Page Not Found)** page instead of AWS’s default error page:

1️⃣ Scroll down to **“Custom Error Responses”**  
2️⃣ Click **"Create Custom Error Response"**  
3️⃣ Enter **HTTP Error Code: 404**  
4️⃣ Enter **Response Page Path:** `/404.html` (if you have a custom 404 page).  
5️⃣ Set **TTL (Time to Live) to 60 seconds** (for quick updates).

✅ This ensures visitors see a friendly 404 error page instead of an AWS error message.

---

### **🛠️ Step 7: Create the CloudFront Distribution**

🚀 **Final step!**

* Scroll to the bottom and click **“Create Distribution”**.
    
* It will take **15-30 minutes** for CloudFront to deploy.
    

🔹 **How to Check if it Works?**  
1️⃣ Copy the **CloudFront Distribution URL** (e.g., `d3abcd123.cloudfront.net`).  
2️⃣ Paste it in your browser.  
3️⃣ If your website loads with HTTPS (`https://d3abcd123.cloudfront.net`), **it’s working!** 🎉

---

### **🛠️ Step 8: Update Route 53 DNS to Use CloudFront**

Now, let’s connect our domain (e.g., `example.com`) to CloudFront:

1️⃣ Open **AWS Route 53**  
2️⃣ Go to **Hosted Zones** → Select your domain  
3️⃣ Click **"Create Record"** → Select **A Record**  
4️⃣ Select **Alias → Yes**  
5️⃣ Choose **CloudFront Distribution** from the dropdown  
6️⃣ Click **"Save"**

✅ After 24 hours, your website will be **secure (HTTPS) and globally fast (CDN-enabled)!** 🚀

---

## **8️⃣ Conclusion**

🎉 **Congratulations!** You have successfully:  
✅ Secured your website with **SSL (HTTPS)** using **AWS CloudFront**.  
✅ Improved **website speed & performance** with **CDN caching**.  
✅ Configured **Route 53** for a fully optimized website.

🔗 **Next Steps?**

* **Optimize Images** for better performance.
    
* **Enable Logging & Monitoring** for analytics.
    
* **Explore AWS WAF** for security protection.