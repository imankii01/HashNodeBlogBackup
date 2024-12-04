---
title: "🚀 Day 4: Mastering EC2 Instances – Launch, Secure, and Deploy MERN Stack on AWS"
seoTitle: "Launch, Secure, and Deploy MERN Stack on EC2 – Deep Dive into AWS EC2 "
seoDescription: "Learn to launch and secure EC2 instances on AWS, and deploy a full MERN stack application (MongoDB, Express.js, React.js, Node.js) on EC2. Step-by-step tuto"
datePublished: Wed Dec 04 2024 05:59:47 GMT+0000 (Coordinated Universal Time)
cuid: cm49hapdd002q09l37ins947u
slug: day-4-mastering-ec2-instances-launch-secure-and-deploy-mern-stack-on-aws
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1733291937084/c2a1da0b-5245-4253-b82f-76a02dcda899.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1733291959959/6e9d19d3-ac21-448e-b895-728c5a00061e.png
tags: cloud-computing, aws-ec2-setup, nodejs-on-aws, ec2-tutorial, deploy-mern-stack, aws-ec2-for-beginners, ec2-ssh-security, aws-key-management, mongodb-on-ec2, mern-stack-tutorial, aws-cloud-tutorial

---

Welcome to **Day 4** of my **AWS Learning Series**, where we’ll dive deeply into **Amazon EC2 (Elastic Compute Cloud)**, which is one of the most essential services for deploying applications and managing compute resources on the cloud. In this detailed tutorial, we will explore how to **launch**, **secure**, and **configure** EC2 instances on AWS. Additionally, we’ll cover a practical **step-by-step guide to deploying a MERN stack project** (MongoDB, Express.js, React.js, and Node.js) on your EC2 instance.

We will also explore essential security practices that ensure your EC2 instances are not only functional but also secure, protecting them from unauthorized access. By the end of this blog, you will be well-equipped to manage and deploy applications on EC2, and understand the fundamentals of securing them.

---

### **What is EC2?**

**Amazon EC2** (Elastic Compute Cloud) is a service that allows you to run virtual machines (known as **instances**) on the AWS cloud. These instances can be configured with a wide range of operating systems, computing resources (CPU, memory, storage), and network configurations, making it ideal for a variety of use cases—from simple static websites to complex machine learning models and large-scale production systems.

EC2 offers great flexibility in terms of instance types, storage options, and pricing models, allowing you to tailor your cloud infrastructure to your specific needs. With the ability to scale your EC2 instances up or down depending on the workload, you can efficiently manage costs and performance.

### **Types of EC2 Instances**

Before we begin the process of launching your first EC2 instance, it’s important to understand the different **instance types** available in EC2:

1. **General Purpose Instances** (e.g., t2.micro, t3.medium):
    
    * **Ideal for**: Small to medium web servers, testing, and development environments.
        
    * These instances provide a balance of computing, memory, and networking resources.
        
2. **Compute Optimized Instances** (e.g., c5.large):
    
    * **Ideal for**: CPU-intensive applications like batch processing and web servers requiring high-performance CPUs.
        
3. **Memory Optimized Instances** (e.g., r5.large):
    
    * **Ideal for**: Memory-intensive applications such as real-time big data analytics, in-memory caches, and high-performance databases.
        
4. **Storage Optimized Instances** (e.g., i3.xlarge):
    
    * **Ideal for**: Applications requiring high, sequential read and write access to very large data sets.
        
5. **GPU Instances** (e.g., p3.2xlarge):
    
    * **Ideal for**: Machine learning, scientific computing, and video rendering.
        

Choosing the right instance type is crucial for performance and cost management. For this tutorial, we will focus on the **t2.micro** instance, which is eligible for the AWS Free Tier and perfect for smaller applications and testing purposes.

---

### **1️⃣ Launching Your First EC2 Instance on AWS**

Now that we understand what EC2 is, let’s walk through the **step-by-step process** of launching an EC2 instance.

#### Step 1: Sign in to AWS Console

* Visit the [**AWS Management Console**](https://aws.amazon.com/console/) and log in with your AWS credentials. If you don’t have an account, you’ll need to sign up and create one.
    

#### Step 2: Navigate to EC2

* On the AWS Console homepage, type **“EC2”** in the search bar and select **EC2** under the "Compute" services. This will take you to the EC2 dashboard.
    

#### Step 3: Click “Launch Instance”

* On the EC2 Dashboard, click the **Launch Instance** button to create a new virtual machine (EC2 instance).
    

#### Step 4: Select an Amazon Machine Image (AMI)

* AWS provides different **pre-configured AMIs** for EC2 instances. You can choose from Linux-based operating systems like **Amazon Linux 2**, **Ubuntu**, **Red Hat**, or Windows-based AMIs.
    
* For this tutorial, let’s choose **Ubuntu Server** as the operating system. It’s widely used for deploying applications and web services.
    

#### Step 5: Choose Instance Type

* The next screen allows you to select an instance type. As we are using the **AWS Free Tier**, we will choose the **t2.micro** instance type, which is sufficient for testing and small applications.
    

#### Step 6: Configure Instance

* You can configure the number of instances, network settings, and other advanced configurations here. For beginners, the default settings are typically fine.
    
* Ensure that the **VPC** (Virtual Private Cloud) and **Subnet** are set to the default settings unless you’re working with a more advanced configuration.
    

#### Step 7: Add Storage

* By default, EC2 instances come with a **30GB EBS** volume. If you require more storage, you can add additional volumes in this step.
    
* You can also choose the **volume type**, such as **General Purpose SSD** (gp2), for better performance.
    

#### Step 8: Configure Security Group

* A **Security Group** acts as a virtual firewall to control traffic to and from your EC2 instance.
    
* Create a new security group with the following rules:
    
    * **SSH (Port 22)**: Allow access from your IP address (for remote login).
        
    * **HTTP (Port 80)**: Allow incoming traffic from all IP addresses (for serving web applications).
        
    * **HTTPS (Port 443)**: Allow incoming traffic from all IP addresses (for secure communication).
        

#### Step 9: Create a Key Pair

* A **key pair** is necessary to SSH into your EC2 instance. If you don’t have one, create a new key pair and download the **.pem** file to your local system.
    
* **Important**: Keep this file secure. You will need it to connect to your EC2 instance via SSH.
    

#### Step 10: Launch the Instance

* After reviewing your instance configuration, click **Launch**. Your instance will begin the boot-up process.
    

---

### **2️⃣ Securing Your EC2 Instance**

Now that your EC2 instance is up and running, it’s crucial to secure it properly. A few simple steps can help safeguard your EC2 instance from unauthorized access.

#### 2.1: Connecting to EC2 Using SSH

To connect to your EC2 instance, you need to use SSH (Secure Shell) with the private key (`.pem`) file that you downloaded during the instance creation process.

1. **Locate the Public IP**:
    
    * From the EC2 dashboard, find your instance, and note its **Public IP** or **Public DNS**.
        
2. **Set File Permissions**:
    
    * Ensure that your **.pem** file has the correct permissions. Run the following command in your terminal:
        
        ```bash
        chmod 400 /path/to/your-key.pem
        ```
        
3. **SSH Command**:
    
    * Use the following SSH command to connect to your instance:
        
        ```bash
        ssh -i /path/to/your-key.pem ubuntu@your-ec2-public-ip
        ```
        
        Replace `/path/to/your-key.pem` with the path to your private key and `your-ec2-public-ip` with the actual public IP of your instance.
        

#### 2.2: Configuring Security Groups

Security Groups are crucial for controlling the **inbound** and **outbound** traffic to your EC2 instance. Here are some common security group configurations for your instance:

* **Allow SSH** (Port 22) for remote access: Restrict this to your IP address to prevent unauthorized access.
    
* **Allow HTTP** (Port 80) for web traffic: Open this port for everyone if you're running a public-facing web server.
    
* **Allow HTTPS** (Port 443) for secure web traffic: Open this port to ensure encrypted communication with your server.
    
* **Allow custom ports**: If you're running additional services, such as a database, you may need to open other ports.
    

#### 2.3: Key Pair Management

When accessing your EC2 instance via SSH, the key pair is used to authenticate you. It is essential to securely store the **private key** and never share it. If the private key is compromised, you may want to regenerate the key pair or **disable access** using the compromised key.

---

### **3️⃣ Deploying MERN Stack Project on EC2**

In this section, we’ll take you through the process of deploying a **MERN stack application** on your EC2 instance.

#### 3.1: Pre-Requisites

* Ensure that **Node.js**, **npm**, and **Git** are installed on your EC2 instance. You will also need to configure **MongoDB** for your database (either self-hosted or using **MongoDB Atlas**).
    

#### 3.2: Installing Node.js and npm

1. **Update Package Lists**:
    
    ```bash
    sudo apt update
    ```
    
2. **Install Node.js**:
    
    ```bash
    sudo apt install nodejs npm
    ```
    
3. **Verify Installation**:
    
    ```bash
    node -v
    npm -v
    ```
    

#### 3.3: Install Git (if using

GitHub)

1. **Install Git**:
    
    ```bash
    sudo apt install git
    ```
    
2. **Clone Your MERN Project**:
    
    ```bash
    git clone https://github.com/your-username/your-mern-project.git
    cd your-mern-project
    ```
    

#### 3.4: Install Dependencies

Inside your project folder, run the following command to install all necessary dependencies for both the front-end (React.js) and back-end (Node.js/Express.js):

```bash
npm install
cd client
npm install
```

#### 3.5: Set Up MongoDB

You can either:

1. **Install MongoDB on EC2**:
    
    * Follow the official MongoDB installation guide to set up MongoDB locally on your EC2 instance.
        
2. **Use MongoDB Atlas**:
    
    * Configure your **MongoDB URI** to point to a **MongoDB Atlas** instance. This is a more scalable solution for cloud applications.
        

#### 3.6: Start the Application

1. **Start the Backend (Express.js)**:
    
    ```bash
    npm start
    ```
    
2. **Start the Frontend (React.js)**:
    
    ```bash
    cd client
    npm start
    ```
    

#### 3.7: Set Up Reverse Proxy with Nginx (Optional)

If you want to handle web traffic more efficiently, you can use **Nginx** as a reverse proxy:

1. **Install Nginx**:
    
    ```bash
    sudo apt install nginx
    ```
    
2. **Configure Nginx**: Edit the **nginx.conf** to set up a reverse proxy to your Node.js server.
    
    ```bash
    sudo nano /etc/nginx/sites-available/default
    ```
    
    Example configuration:
    
    ```nginx
    server {
        listen 80;
        server_name your-ec2-public-ip;
    
        location / {
            proxy_pass http://localhost:3000;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }
    ```
    
3. **Restart Nginx**:
    
    ```bash
    sudo service nginx restart
    ```
    

#### 3.8: Access the Application

Once the application is deployed, you can access it by entering the **Public IP** of your EC2 instance in your browser.

---

### **Conclusion**

In **Day 4**, we’ve covered everything you need to know to launch, configure, and secure **EC2 instances** on AWS. You’ve also learned how to deploy a **MERN stack** project on EC2, ensuring that your web application is live and accessible to the world.

We’ve also discussed security best practices for SSH, Security Groups, and Key Pair Management to ensure your EC2 instances remain secure.

Stay tuned for **Day 5**, where we’ll dive into **load balancing** and **auto-scaling EC2 instances** for high availability.