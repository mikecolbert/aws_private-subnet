# aws_private-subnet
Create public and private subnets inside of a named VPC. After creating the subnets you will create EC2 instances in each and connect to them via SSH. The instance in your public subnet will act as a bastion host, allowing you to connect to the EC2 instance in the private subnet.

### Step 1: Create a VPC
Create a VPC.

![Creating a VPC](https://github.com/mikecolbert/aws_private-subnet/raw/main/images/create-vpc.jpg?raw=true)

A. From the AWS console, under **Networking and Content Delivery**, choose **VPC**
B. In the left menu, click **Your VPCs** and then choose **Create VPC**
C. For **Name tag** enter *hawkid_vpc* (i.e. colbert_vpc)
D. For **IPv4 CIDR block**, use *10.0.0.0/16*
E. **Tenancy** should be left as *Default*
F. Click **Create VPC**

***

### Step 2: Create a public subnet
Create a public subnet.


### Step 3: Create a private subnet
Create a private subnet.

### Step 4: Create a public route table
Create a public route table.

### Step 5: Create a private route table
Create a private route table.

### Step 6: Create an Internet gateway
Create an Internet gateway.

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






