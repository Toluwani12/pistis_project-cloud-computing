# AWS Lab Guide Project

### 1)  What is EC2?
Explain the purpose of Amazon EC2. Why is it considered an example of Infrastructure as a Service (IaaS)?

**Answer:**

Amazon EC2 (Elastic Compute Cloud) is a web service provided by AWS (Amazon Web Services) that allows users to launch and manage virtual servers, known as instances, in the cloud. It provides scalable computing capacity, which means you can quickly scale resources up or down based on your needs.

## Purpose of Amazon EC2:
* Run Applications in the Cloud: EC2 lets you deploy web applications, host APIs, run background jobs, and more.

* Scalability: Easily increase or decrease the number and size of instances to match your demand.

* Cost-Effective: You pay only for what you use (on-demand, reserved, or spot pricing).

## EC2 is considered IaaS because:

* It provides virtualized computing resources over the internet (like servers, storage, and networking).

* AWS manages the underlying hardware and hypervisor infrastructure for you.

### 2)  Launching EC2 Instance
#### Step 1: Launch an EC2 Instance
* I signed in to the AWS Management Console,  searched for EC2, clickED on Instances and clicked on Launch Instances.

    ![alt text](Img/launch.png)

* I chose an Amazon Machine Image (AMI) and selected Ubuntu and for an Instance Type, I selected t3.micro (free tier eligible) for basic purposes.

    ![alt text](Img/AMI.png)

* I then completed the remaining config,reviewed and launched the instance.

    ![alt text](Img/sucess-instance.png)

#### Step 2: Connect to the EC2 Instance
* Connected to the instance via SSH:
    ```bash
    ssh -i ~/test.pem ubuntu@ec2-16-170-172-159.eu-north-1.compute.amazonaws.com
    ```

#### Step 3: Installing a Web Server (Nginx)
* I installed Nginx using the command:
    ```bash
    sudo apt update               # Update package list on Ubuntu
    sudo apt install nginx -y      # Install Nginx on Ubuntu
    sudo systemctl start nginx     # Start Nginx service
    sudo systemctl enable nginx    # Ensure Nginx starts on boot
    ```
    I confirmed it was working on my browser
    ![alt text](Img/nginx-browser.png)

### 3)  Understanding Security Groups
#### Step 1  Set Up Security Groups
* SSH (Port 22): Allow inbound SSH traffic from your IP to connect via SSH.
* HTTP (Port 80): Allow inbound HTTP traffic from anywhere to serve your web server.
* HTTPS (Port 443): Allow inbound HTTPS traffic from anywhere to serve your web server.

    I confirmed and saw the port were opened 

    ![alt text](Img/ssh-port.png)

#### Step 2: Purpose of Security Groups
* Security Groups act as virtual firewalls for your EC2 instance.

* They control inbound and outbound traffic to and from your instance.
* Security groups are stateful, meaning that if you allow inbound traffic, the response to that traffic is automatically allowed (you don't need to explicitly allow outbound responses).

### 4) Elastic IPs
#### Step 1: Allocating and Assigning an Elastic IP
* On the EC2 dashboard, I clicked on `Elastic IPs` under Network & Security.

* Then `Allocate Elastic IP Address` and chose the default Amazon pool.
![alt text](Img/allocate%20Ip%20add.png)
* I then clicked on Allocate.
* After allocation, I selected the Elastic IP and click `Actions → Associate Elastic IP Address.` and chose the EC2 instance I launched earlier from the Instance dropdown and clicked `Associate.`

#### Step 2: Why Use Elastic IPs?
* Persistent IP Address
* Easier DNS Management
    * Since the IP doesn’t change, you don’t need to constantly update DNS records.
* Failover & High Availability
    * You can quickly remap an Elastic IP to a different EC2 instance if the original one fails.
* Better Control Over Network Addressing
* Supports Load Balancing & Scaling

### 5) Explain the difference between stopping, starting, and terminating an EC2 instance. Perform each operation and observe the effects.

**Answer:**

1. Instance <mark>shuts down</mark> is like powering off a computer. The `EBS (Elastic Block Store)` volumes are preserved, the instance remains in your account and can be restarted later, the `public IP (non-Elastic)` is released and the `elastic IP` (if attached) is retained.

2. Starting an EC2 Instance means booting up a previously stopped instance.If no Elastic IP was used, a new public IP is assigned.Instance retains its data on the EBS root volume.Instance ID remains the same.

3. Terminating an EC2 Instance means Instance is permanently deleted.All attached EBS volumes are deleted, unless marked as "Delete on Termination = false". Instance ID is lost forever. Elastic IP (if any) is disassociated but not deleted — you must release it manually.

### 6) Outline steps that a junior DevOps engineer can take to identify and resolve performance issues on EC2 instances, focusing on monitoring, analysis, and optimization techniques.

**Answer:**

*  **Check CloudWatch metrics** (CPU, disk, network)
*  **Check EC2 status checks** (system & instance)
*  **Review logs** (CloudWatch or `/var/log/` on instance)
*  **SSH into instance** and run:

  * `top` or `htop` – check CPU/memory usage
  * `df -h` – check disk space
  * `free -m` – check RAM
  * `iotop` – check disk I/O
*  **Fix issues**:

  * Restart heavy services
  * Clean up disk space (old logs, temp files)
  * Kill memory-hogging processes
*  **Scale** if needed:

  * Scale up (bigger instance type)
  * Scale out (add more instances + Load Balancer)
*  **Set up CloudWatch alarms** for early warnings
*  **Use Grafana dashboards** for better visualization


