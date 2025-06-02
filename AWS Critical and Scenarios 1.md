# AWS Critical Thinking Projects and Questions

## Scenario 1:
As a junior DevOps engineer, you are tasked with designing a solution to ensure users in two different geographical locations, India and London, have a good browsing experience with minimal latency. Create an infrastructure solution that optimizes performance and minimizes latency for users in both regions. Consider factors such as content delivery networks (CDNs), edge caching, global load balancing, and region-specific deployment strategies to achieve this goal effectively. After designing the solutions (use draw.io), implement them and create the infrastructure in your AWS console.

**Answer:**
#### Step 1: Deploy App in Two Regions
*  I launched the app in Mumbai (ap-south-1) and London (eu-west-2) using EC2.
![alt text](Img/india.png)

    ![alt text](Img/London.png)

*  I SSH into them and installed web servers using this commands
    ```bash
    sudo apt-get update -y
    sudo apt-get install -y apache2
    sudo systemctl start apache2
    sudo systemctl enable apache2
    sudo bash -c 'echo "<h1>Hello from Mumbai!</h1>" > /var/www/html/index.html'
    ```

#### Step 2: Set Up Auto Scaling and Load Balancer in Both Regions

* GOn the EC2 dashboard, I clicked the Load Balancers

* Create an Application Load Balancer:
    *  Internet-facing, IPv4
    *  Add listener on port 80
    *  Attach to same VPC as your EC2
    *  Add one or more subnets
    *  Register your EC2 instance(s)
* Create a Target Group for the ALB and register instances
![alt text](Img/London-Target-group.png)
![alt text](Img/India-Target-group.png)
* I set up Auto Scaling Groups:
    * I created a Launch Template with EC2 config where I defined:
        * AMI ID (Ubuntu)
        * Instance type (t2.micro)
        * Key pair
        * Security groups (allow HTTP/HTTPS, SSH)
        * User Data script (for auto-installing Apache and webpage)

    * To configure capacity - Set min=1, max=3 instances
    *  Attach to target group from ALB

        ![alt text](Img/India-ASG.png)

        ![alt text](Img/London-ASG.png)

* I attached the ASG TO THE LB after it was created by clicking on the Lb in each region, clicked on Intergration and added the LB

* I Tested in Browser
* On the EC2 dsahboards → Load Balancers, I copied the DNS name of the ALB (india-lb-2123463983.ap-south-1.elb.amazonaws.com/)

    ![alt text](Img/india-site.png)


## Scenario 2:
As a junior DevOps engineer, explain how you would host a static website in an Amazon S3 bucket. Describe the steps in configuring the S3 bucket for website hosting, uploading the static website files, setting permissions, and enabling static website hosting. Additionally, discuss any considerations for domain configuration and CDN integration to enhance website performance. After proper research, provide an architectural diagram and host a sample static website in an Amazon S3 Bucket following the solutions and steps you have come up with.

**Answer:**
#### Step 1: Creating the S3 Bucket
* On the S3 Console → I clicked Create Bucket and named it

* I uncheck Block all public access 
* Then left rest as default → Create bucket
![alt text](Img/bucket.png)

#### Step 2: Create the Website Files 

1. Used this command to create a basic HTML file:
    ```bash
    echo '<!DOCTYPE html>
    <html>
    <head><title>My Static Website</title></head>
    <body>
    <h1>Hello from my S3 static website!</h1>
    <p>Welcome to the demo site.</p>
    </body>
    </html>' > index.html
    ```
2. Added an error.html page
    ```bash
    echo '<html><body><h1>404 - Page Not Found</h1></body></html>' > error.html
    Now you have a basic site ready.
    ```
#### Step 3: Upload Files to S3
* Open your bucket

* Click `Upload` and upload both index.html and error.html
* Click Upload

#### Step 4: Make Files Public by Adding a Bucket Policy:
* I went to \ bucket → Permissions tab → Bucket policy, then pasteed this 
    ```json
    {
    "Version": "2012-10-17",
    "Statement": [
        {
        "Sid": "PublicReadGetObject",
        "Effect": "Allow",
        "Principal": "*",
        "Action": "s3:GetObject",
        "Resource": "arn:aws:s3:::tolu-test/*"
        }
    ]
    }
    ```
#### Step 5: Enable Static Website Hosting
*   On the Properties tab, I scrolled to `Static website hosting` and chose `Enable`

* I Set:
    * Index document: index.html
    * Error document: error.html
* Saved the settings

Scrolling down to Static Website Hosting again to see Website endpoint URL,

```arduino
http://tolu-test.s3-website-us-east-1.amazonaws.com
```
![alt text](Img/result-s3.png)


## Scenario 3:

After analyzing the existing on-premises infrastructure for legacy applications, propose a cloud-native solution that addresses identified challenges, such as single points of failure, scalability limitations, security vulnerabilities, and inefficient resource allocation.

The solution should include:

Separating the database from the application by migrating applications to the cloud while keeping the database on-premises.
Implementing robust security measures for data in transit and at rest.
Eliminating single points of failure through redundancy and high availability.

**Answer:**

Proposed Cloud Architecture:
1. Lift-and-Shift App to AWS
Host the application on EC2, ECS, or Elastic Beanstalk

Use Auto Scaling Group (ASG) and ALB for scalability and high availability

2. Keep DB On-Premises
Use AWS Direct Connect (for low-latency, secure network link) or VPN over AWS Site-to-Site

Application securely connects to the on-premises DB via VPC + VPN/Direct Connect

3. Security for Data in Transit & At Rest
Use SSL/TLS encryption between app and database

Encrypt EC2 volumes with EBS encryption

Use IAM roles and Security Groups to restrict access

4. Redundancy and High Availability
Deploy app instances across multiple Availability Zones

Use Auto Scaling to replace failed instances

Place ALB in front to distribute traffic













# To note
VPC: A VPC is like a reserved table in a crowded restaurant. It's a private and secure space within a public cloud, allowing organizations to host their resources without sharing them with other users.

Subnets: Determine the specific AZs and networking placement of instances.A subnet, in simple terms, is a way to divide a large network into smaller, more manageable parts, like breaking a big classroom into smaller study groups. This helps with network traffic, security, and organization

Capacity: Controls scaling behavior (how many instances run at start and limits).

 
