---
title: "Step-by-Step Guide to Full-Stack E-Commerce App Development"
seoTitle: " Building a Full-Stack E-Commerce Application with AWS EC2, RDS, and S"
seoDescription: "learn how to integrate AWS EC2, RDS, and S3 to build a full-stack e-commerce application. This step-by-step guide walks you through deploying Node.js on EC2"
datePublished: Sat Dec 07 2024 17:01:59 GMT+0000 (Coordinated Universal Time)
cuid: cm4ef9u7a000309mkfmc5ew2s
slug: step-by-step-guide-to-full-stack-e-commerce-app-development
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1733590887324/20cffa3c-0910-449a-bd74-dc58c784b2a7.webp
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1733590845232/167952fc-3685-412f-b5a9-546e2afb700a.webp
tags: aws, projects, aws-lambda, aws-sdk, aws-cdk, aws-deployment

---

**Day 1 to Day 6**, let's dive deeper into the project process. The goal is to build an **E-Commerce application** that uses **AWS EC2** to host the application, **Amazon RDS** to store product information in a relational database, and **Amazon S3** to handle product image uploads.

This project will be a simple e-commerce platform where users can:

* View a list of products
    
* Add new products to the store (with product image uploads)
    
* View product details, including images stored on **Amazon S3**
    
* Use **Amazon RDS** to manage product data (name, price, description)
    

You will host the application on **AWS EC2** and store data on **Amazon RDS** (MySQL or PostgreSQL). We'll also use **AWS S3** for product image uploads. Let's break this down into a detailed step-by-step guide.

---

### **Step 1: Setting Up AWS EC2 to Host the Application**

#### **1.1 Launch an EC2 Instance**

1. Go to the **EC2 Dashboard** on your AWS Management Console.
    
2. Click **Launch Instance**.
    
3. Select **Amazon Linux 2 AMI** or **Ubuntu** (depending on your preference).
    
4. Choose an **Instance Type**: For a basic project, you can use the **t2.micro** instance (eligible for the free tier).
    
5. Configure the instance settings and ensure the **Security Group** allows:
    
    * SSH (port 22) for connecting to the instance.
        
    * HTTP (port 80) to allow web traffic.
        
6. Create and download an **SSH key pair** to access the EC2 instance.
    

#### **1.2 Connect to EC2 Instance**

Use SSH to connect to your EC2 instance:

```bash
ssh -i /path/to/your-key.pem ec2-user@your-ec2-public-ip
```

#### **1.3 Install Node.js and Required Packages**

On your EC2 instance, install **Node.js** and **npm**:

```bash
# Update the system
sudo yum update -y

# Install Node.js
sudo yum install -y nodejs npm

# Verify the installation
node -v
npm -v
```

#### **1.4 Set Up Your Node.js Project**

1. SSH into your EC2 instance.
    
2. Create a new directory for your project:
    
    ```bash
    mkdir ecommerce-app
    cd ecommerce-app
    ```
    
3. Initialize the Node.js project:
    
    ```bash
    npm init -y
    ```
    
4. Install **Express**, **MySQL** (or **PostgreSQL**), **AWS SDK**, and other dependencies:
    
    ```bash
    npm install express mysql2 aws-sdk body-parser cors dotenv
    ```
    

---

### **Step 2: Set Up Amazon RDS for Product Data**

#### **2.1 Create a Database Instance in RDS**

1. Go to **RDS** in the AWS console and click **Create Database**.
    
2. Choose **MySQL** or **PostgreSQL** (based on your preference).
    
3. Select the **Free Tier** for cost-saving.
    
4. Set the database name, master username, and password.
    
5. Set the VPC and security group to allow inbound traffic from your EC2 instance.
    

#### **2.2 Connect to RDS from EC2**

Once your RDS instance is ready, note the **endpoint** (hostname) and **port**. Install a MySQL or PostgreSQL client on your EC2 instance and test the connection.

For MySQL, in your Node.js app, create the database connection like so:

```javascript
const mysql = require('mysql2');
const connection = mysql.createConnection({
  host: 'your-rds-endpoint',
  user: 'your-db-username',
  password: 'your-db-password',
  database: 'your-db-name',
});

connection.connect((err) => {
  if (err) {
    console.error('Error connecting to database:', err.stack);
    return;
  }
  console.log('Connected to RDS database');
});
```

#### **2.3 Create a Products Table in MySQL**

Using a MySQL client or through Node.js, create a **products** table:

```sql
CREATE TABLE products (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  price DECIMAL(10, 2) NOT NULL,
  description TEXT,
  image_url VARCHAR(255)
);
```

---

### **Step 3: Set Up Amazon S3 for Image Storage**

#### **3.1 Create an S3 Bucket**

1. Go to the **S3 Dashboard** on AWS and create a new bucket.
    
2. Enable **public read** for the bucket or configure appropriate permissions.
    
3. Enable **versioning** and **encryption** (optional but recommended for security).
    

#### **3.2 Install AWS SDK**

In your Node.js application, install the **AWS SDK**:

```bash
npm install aws-sdk
```

#### **3.3 Configure AWS SDK for Image Uploads**

Set up the AWS SDK in your app:

```javascript
const AWS = require('aws-sdk');
const s3 = new AWS.S3({
  accessKeyId: 'your-access-key',
  secretAccessKey: 'your-secret-key',
  region: 'your-region'
});
```

#### **3.4 Implement Image Uploads**

In your Express API, add an endpoint to handle image uploads to S3:

```javascript
const fs = require('fs');

const uploadImageToS3 = async (filePath, fileName) => {
  const params = {
    Bucket: 'your-s3-bucket-name',
    Key: fileName,
    Body: fs.createReadStream(filePath),
    ACL: 'public-read',
  };

  try {
    const data = await s3.upload(params).promise();
    console.log('File uploaded successfully:', data.Location);
    return data.Location;
  } catch (error) {
    console.error('Error uploading file:', error);
    throw error;
  }
};
```

---

### **Step 4: Implement Node.js API**

#### **4.1 Set Up Express Server**

Set up the basic Express server:

```javascript
const express = require('express');
const app = express();
const bodyParser = require('body-parser');
app.use(bodyParser.json());
const port = 3000;

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```

#### **4.2 Create CRUD Routes for Products**

* **GET**: Fetch all products
    
* **POST**: Add a new product (with image upload to S3)
    
* **PUT**: Update product details
    
* **DELETE**: Remove a product
    

For example, for adding a product with an image upload:

```javascript
app.post('/products', async (req, res) => {
  const { name, price, description, imagePath } = req.body;
  
  try {
    // Upload image to S3
    const imageUrl = await uploadImageToS3(imagePath, `${Date.now()}-${name}.jpg`);
    
    // Insert product into RDS
    const query = 'INSERT INTO products (name, price, description, image_url) VALUES (?, ?, ?, ?)';
    connection.query(query, [name, price, description, imageUrl], (err, results) => {
      if (err) {
        res.status(500).send('Error saving product');
        return;
      }
      res.status(201).send('Product added successfully');
    });
  } catch (error) {
    res.status(500).send('Error uploading image or saving product');
  }
});
```

---

### **Step 5: Create Frontend (HTML/CSS/JS)**

#### **5.1 Basic HTML Form**

Create a basic HTML form to upload products:

```html
<form id="productForm">
  <input type="text" id="productName" placeholder="Product Name" required>
  <input type="number" id="productPrice" placeholder="Price" required>
  <textarea id="productDescription" placeholder="Description" required></textarea>
  <input type="file" id="productImage" required>
  <button type="submit">Add Product</button>
</form>
```

#### **5.2 JavaScript to Handle Form Submission**

Use **JavaScript (AJAX)** to submit the form data and handle image uploads:

```javascript
document.getElementById('productForm').addEventListener('submit', async function (e) {
  e.preventDefault();
  
  const name = document.getElementById('productName').value;
  const price = document.getElementById('productPrice').value;
  const description = document.getElementById('productDescription').value;
  const image = document.getElementById('productImage').files[0];
  
  const formData = new FormData();
  formData.append('name', name);
  formData.append('price', price);
  formData.append('description', description);
  formData.append('image', image);

  const response = await fetch('http://your-ec2-public-ip/products', {
    method: 'POST',
    body: formData,
  });

  if (response.ok) {
    alert('Product added successfully!');
  } else {
    alert('Failed to add product.');
  }
});
```

---

### **Step 6: Deploying the App on EC2**

1. Push your **Node.js app** to your EC2 instance using **Git** or **SCP**.
    
2. Run the app on EC2:
    
    ```bash
    node app.js
    ```
    
3. Make sure your EC2 **Security Group** allows HTTP traffic on port 80.
    

---

### **Next Steps for Advanced Features**

* **Add user authentication** using JWT (JSON Web Tokens).
    
* **Set up load balancing and auto-scaling** for handling traffic.
    
* **Integrate Amazon SES** for sending order confirmation emails.