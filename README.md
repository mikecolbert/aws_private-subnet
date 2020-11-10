# Creating Public and Private Subnets in AWS
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
Tell the public route table to use your new internet gateway to go to the Internet.

* A. From the VPC console, click **Route Tables** in the left menu
* B. Click your public route table to choose it
* C. In the route table details below, choose the **Routes** tab.
* D. Click **Edit routes**
* E. Click **Add route**. Destination = 0.0.0.0/0 and Target = internet gateway, then choose your gateway
* F. Choose **Save routes**
* G. Choose the **Subnet Associations** tab. Click **Edit subnet associations**.
* H. Choose your **public** subnet and click **Save**

***

### Step 8: Create a NAT gateway
Create a NAT gateway in the public subnet.

![Creating a NAT gateway](/images/create-nat-gateway.jpg)


* A. From the VPC console, click **NAT Gateways** in the left menu
* B. Click **Create NAT gateway**
* C. For **Name tag** enter *hawkid-nat-gw* (i.e. colbert-nat-gw)
* D. For **Subnet** choose your public subnet
* E. Click **Allocate Elastic IP** to create an Elastic IP for the gateway
* F. Choose **Create NAT gateway**

* G. After the NAT gateway is created, refresh the page and select it.
* H. On the **Details** tab below, click on the **Network Interface ID** link
* I. When the network interface opens, edit the name to be *hawkid-nat-net-if* (i.e. colbert-nat-net-if)
* J. In the **Details** tab below, click the **IPv4 Public IP** IP address. A popup will open, click the IP address again.
* K. Now that the Elastic IP addresses window is open, name your Elastic IP as *hawkid-nat-eIP* (i.e. colbert-nat-eIP)

***

### Step 9: Edit private route table
Tell private route table to use NAT gateway.

* A. From the VPC console, click **Route Tables** in the left menu
* B. Click your private route table to choose it
* C. In the route table details below, choose the **Routes** tab.
* D. Click **Edit routes**
* E. Click **Add route**. Destination = 0.0.0.0/0 and Target = NAT Gateway, then choose your NAT gateway
* F. Choose **Save routes**
* G. Choose the **Subnet Associations** tab. Click **Edit subnet associations**.
* H. Choose your **private** subnet and click **Save**

***

### Step 10: Create a public server
Create a public Linux EC2 instance in the **public** subnet. 

During Step 3: Configure Instance Details make the following changes:
* A. Network: *choose your VPC*
* B. Subnet: *choose your public subnet*
* C. Auto-assign Public IP: *Enable*

During Step 5: Add Tags name your server
* D. **Add Tags** -- Key: Name ; Value: *hawkid-public-svr* (i.e. colbert-public-svr)

During Step 6: Configure Security Group
* E. Name: *hawkid-public-svr-sg* (i.e. colbert-public-svr-sg)
* F. Description: *hawkid VPC public server security group*
* G. SSH - TCP - 22 - My IP -- *your ip address/32* - SSH from my computer

***

### Step 11: Create a private server
Create a Linux EC2 instance in the **private** subnet.

During Step 3: Configure Instance Details make the following changes:
* A. Network: *choose your VPC*
* B. Subnet: *choose your private subnet*
* C. Auto-assign Public IP: *Disable*

During Step 5: Add Tags name your server
* D. **Add Tags** -- Key: Name ; Value: *hawkid-private-svr* (i.e. colbert-private-svr)

During Step 6: Configure Security Group
* E. Name: *hawkid-private-svr-sg* (i.e. colbert-private-svr-sg)
* F. Description: *hawkid VPC private server security group*
* G. SSH - TCP - 22 - custom -- *10.0.1.0/24* - SSH from my public subnet

***

### Step 12: SSH from your computer to your public server

* A. SSH into your **public** server.
* B. If you were successful, your security group and interenet gateway is working correctly.

* C. Run `sudo yum check-update` then `sudo yum update`
* D. If the server updated, you have Internet connectivity outbound and your public route table and internet gateway are working correctly.

* E. Copy the private key from your computer (Mac) to your public EC2 instance.
```
scp -r -i "colbert-vpc.pem" /Users/mikec/Desktop/key/colbert-vpc.pem ec2-user@54.83.101.33:~/ 
```

* E. Copy the private key from your computer (Windows) to your public EC2 instance.
* * i. Download Putty SCP
```
pscp -i c:\users\mikec\Desktop\colbert-vpc.ppk -r -P 22 c:\users\mikec\Desktop\colbert-vpc.pem ec2-user@54.227.106.201:/home/ec2-user
```

***

### Step 13: SSH from the public server to your private server
* A. SSH into your **private** server.
` ssh -i "colbert-vpc.pem" ec2-user@54.227.106.201`
* B. If you were successful, your security group and private route table is working correctly.

* C. Run `sudo yum check-update` then `sudo yum update`
* D. If the server updated, you have Internet connectivity outbound and your private route table and NAT gateway are working correctly.




