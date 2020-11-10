# aws_private-subnet
Create public and private subnets inside of a named VPC. After creating the subnets you will create EC2 instances in each and connect to them via SSH. The instance in your public subnet will act as a bastion host, allowing you to connect to the EC2 instance in the private subnet.

### Step 1: Create a VPC
Create a VPC.

![Creating a VPC](/images/create-vpc.jpg)

* A. From the AWS console, under **Networking and Content Delivery**, choose **VPC**
* B. In the left menu, click **Your VPCs** and then choose **Create VPC**
* C. For **Name tag** enter *hawkid-vpc* (i.e. colbert-vpc)
* D. For **IPv4 CIDR block**, use *10.0.0.0/16*
* E. **Tenancy** should be left as *Default*
* F. Click **Create VPC**

***

### Step 2: Create a public subnet
Create a public subnet inside of your new VPC.

![Creating a public subnet](/images/create-subnet.jpg)

* A. From the VPC console, click **Subnets** in the left menu
* B. Choose **Create subnet**
* C. For **Name tag** enter *hawkid-public* (i.e. colbert-public)
* D. Choose the VPC you created in Step 1.
* E. Choose any availability zone, but **don't** choose *No Preference*
* F. Use *10.0.1.0/24* for **IPv4 CIDR block**
* G. Click **Create**

***

### Step 3: Create a private subnet
Create a private subnet inside of your new VPC.

* A. Choose **Create subnet**
* B. For **Name tag** enter *hawkid-private* (i.e. colbert-private)
* C. Choose the VPC you created in Step 1
* D. Choose a different availability zone than the one you used in Step 2
* E. Use *10.0.2.0/24* for **IPv4 CIDR block**
* F. Click **Create**

***

### Step 4: Create a public route table
Create a public route table.

![Creating a public route table](/images/create-route-table.jpg)

* A. From the VPC console, click **Route Tables** in the left menu
* B. Choose **Create route table**
* C. For **Name tag** enter *hawkid-public-rt* (i.e. colbert-public-rt)
* D. Choose the VPC you created in Step 1.
* E. Click **Create**

***

### Step 5: Create a private route table
Create a private route table.

* A. Choose **Create route table**
* B. For **Name tag** enter *hawkid-private-rt* (i.e. colbert-private-rt)
* C. Choose the VPC you created in Step 1.
* D. Click **Create**

***

### Step 6: Create an Internet gateway
Create an Internet gateway.

![Creating an internet gateway](/images/create-internet-gateway.jpg)

* A. From the VPC console, click **Internet Gateways** in the left menu
* B. Choose **Create internet gateway**
* C. For **Name tag** enter *hawkid-igw* (i.e. colbert-igw)
* D. Click **Create**

* E. Select the internet gateway you just created. 
* F. Under the **Actions** menu, choose **Attach to VPC**
* G. Choose the VPC you created in Step 1 and click **Attach internet gateway**


### Step 7: Edit public route table
Tell pubic route table to use Internet gateway.

### Step 8: Create a NAT gateway
Create a NAT gateway in the public subnet.

### Step 9: Edit private route table
Tell private route table to use NAT gateway.

### Step 10: Create a public server
Create a public Linux EC2 instance. Be sure to include public IP check.
Edit security group

### Step 11: Create a private server
Create a Linux EC2 instance in the private subnet.

### Step 12: SSH from your computer to your public server
Yum check-update and update to prove connectivity
curl https://google.com
Copy the private key from your computer (Mac) to this new EC2 instance.
```
scp -r -i "colbert-vpc.pem" /Users/mikec/Desktop/key/colbert-vpc.pem ec2-user@54.83.101.33:~/ 
```
Also, investigate pass-through SSH

### Step 12: SSH from the public server to your private server
Yum check-update and update to prove connectivity
curl https://google.com






