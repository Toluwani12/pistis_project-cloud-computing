# AWS Critical Thinking Projects and Questions
You are managing the infrastructure for an e-commerce website. During peak shopping hours, the website experiences a significant increase in traffic, leading to a strain on the server resources. To address this, you need to implement dynamic scaling using AWS Auto Scaling to ensure optimal performance and cost-effectiveness.



Project Goals:

Understand the given Architecture Diagram
Create the infrastructure
Ensure your infrastructure is split across multiple zones in a single region
Deploy a simple Node.js Application to the infrastructure
Load test the application
Ensure the Auto-scaling group increases the instance when the CPU usage rises to 80%
Ensure the Auto-scaling group increases the instance when the Memory usage rises to 80%
Ensure the Auto-scaling group scales down when the load decreases
Ensure you DELETE all your resources after you are done.

## **Answer:**
#### Step 1: Set Up the VPC and Subnets
1. On the VPC Dashboard in AWS, I clicked `Launch VPC Wizard`.

2. I chose: VPC with Public and Private Subnets and filled in:
    * Name: test
    * IPv4 CIDR block: Leave default or use 10.0.0.0/16
    * Public subnet CIDR: 10.0.0.0/20
    * Private subnet CIDR: 10.0.128.0/20
3. I selected 2 Availability Zones (us-east-1a and us-east-1b) and checked:

    ✅ “Enable DNS Hostnames”

    ✅ “Enable NAT Gateway”

4. I clicked `Create VPC`

#### Step 2: Prepare EC2 Template (App Blueprint)
* On the EC2 Dashboard, I clicked `Launch Templates` and filled it with: 
    * Name: test
    * Chose Ubuntu
    * Set instance type: t2.micro
    * Add key pair
    * Add Security Group 
    * I added this to the user data :
        ```bash
        #!/bin/bash
        yum update -y
        yum install -y git nodejs npm
        cd /home/ec2-user
        git clone https://github.com/heroku/node-js-sample.git
        cd node-js-sample
        npm install
        nohup node index.js > output.log 2>&1 &
        ```
    ![alt text](Img/launch-temp.png)

#### Step 3: Create a Target Group & Load Balancer
I specified the group details with this details for target group:
* Type: Instance
* Port: 80
* Protocol: HTTP
*  Health Check Path: `/`
*  VPC: test
![alt text](Img/test-vpc.png)

I specified the group details with this details for load balancer:
* Type: ALB (Internet-facing)
* Subnets: public-subnet-1 & public-subnet-2
* Security Group: test
* Listener: HTTP (80)
* Forward to your target group
![alt text](Img/LB.png)

#### Step 4: Auto Scaling Group (ASG)
 * I created a Launch Template with EC2 config where I defined:
    * AMI ID (Ubuntu)
    * Instance type (t2.micro)
    * Key pair
    * Security groups (allow HTTP/HTTPS, SSH)
    * User Data script (for auto-installing Apache and webpage)

    * To configure capacity - Set min=1, max=3 instances
    *  Attach to target group from ALB

        ![alt text](Img/India-ASG.png)

* I attached the ASG TO THE LB after it was created by clicking on the Lb in each region, clicked on Intergration and added the LB

