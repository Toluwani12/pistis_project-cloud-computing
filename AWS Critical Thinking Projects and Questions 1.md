# AWS Critical Thinking Projects and Questions

## Introduction to Cloud Storage:
### 1) What are the different types of cloud storage services (block, file, and object storage)? Which type of storage is Amazon S3?

**Answer:**

1. **Block Storage :** Stores data in fixed-sized blocks, similar to traditional hard drives. Used for databases, virtual machines, and transactional applications.

    Example: AWS EBS (Elastic Block Store), Azure Managed Disks.

2. **File Storage :** Organizes data in a hierarchical file system structure. Suitable for shared applications, enterprise file sharing, and content management.

    Example: AWS EFS (Elastic File System), Azure Files.

3. **Object Storage :** Stores data as objects with metadata and unique identifiers.
Best for large-scale unstructured data like images, backups, and logs.

    Example: Amazon S3, Azure Blob Storage, Google Cloud Storage.

💡 Amazon S3 is an Object Storage service.

### 2) What are the key features of Amazon S3 (durability, availability, scalability, and security)?

**Answer:**

- **Durability:**  
  - 99.999999999% (11 nines) durability, ensuring data is not lost.  
  - Data is replicated across multiple Availability Zones.  

- **Availability:**  
  - Designed for 99.99% availability in Standard Tier.  
  - S3 Intelligent-Tiering automatically moves data to lower-cost tiers.  

- **Scalability:**  
  - Handles any amount of data and scales automatically.  
  - No need for capacity planning.  

- **Security:**  
  - Supports encryption (server-side & client-side).  
  - IAM roles and bucket policies restrict access.  


### 3) How does Amazon S3 differ from other cloud storage services like Google Cloud Storage and Microsoft Azure Blob?  
**Answer:**

| Feature | Amazon S3 | Google Cloud Storage | Azure Blob Storage |
|---------|----------|----------------------|--------------------|
| **Storage Type** | Object Storage | Object Storage | Object Storage |
| **Durability** | 99.999999999% | 99.999999999% | 99.999999999% |
| **Availability** | 99.99% | 99.95% - 99.99% | 99.9% - 99.99% |
| **Tiers** | Standard, Intelligent-Tiering, Glacier | Standard, Nearline, Coldline, Archive | Hot, Cool, Archive |
| **Security** | IAM, Encryption, Bucket Policies | IAM, Encryption, Access Control | IAM, Encryption, Access Control |
| **Use Cases** | Backup, logs, ML data storage | BigQuery integration, archival | Hybrid cloud, backups |

---


### 4) What are the benefits of using Amazon S3 (cost-effectiveness, ease of use, and flexibility)? 
**Answer:**

✅ **Cost-Effective**  
- Pay-as-you-go pricing.  
- Lower-cost storage tiers like Glacier for long-term storage.  

✅ **Ease of Use**  
- Simple web interface and API integration.  
- Compatible with multiple AWS services.  

✅ **Flexibility**  
- Stores any data type (images, videos, backups, logs).  
- Supports versioning, replication, and object tagging.  


### 5) How does Amazon S3 integrate with other AWS services (S3 bucket policies, IAM roles, EC2, CloudFront, and Lambda)?  
**Answer:**

🔹 **S3 Bucket Policies & IAM Roles**  
- Control who can access S3 buckets using policies and permissions.  

🔹 **EC2**  
- Store and retrieve data from S3 using EC2 instances.  

🔹 **CloudFront (CDN)**  
- Distribute S3 content globally with low latency.  

🔹 **AWS Lambda**  
- Automate workflows by triggering functions when an object is uploaded.  

🔹 **AWS Backup & Glacier**  
- Move data to long-term storage automatically.  

### 6) What are the best practices for using Amazon S3 (data encryption, access control, and data lifecycle management)? 
**Answer:**

🔐 **Data Encryption**  
- Use **SSE-S3, SSE-KMS, or SSE-C** for server-side encryption.  
- Use **client-side encryption** for additional security.  

🔒 **Access Control**  
- Implement **least privilege** using IAM policies.  
- Use **bucket policies & access control lists (ACLs)** to restrict access.  

♻️ **Data Lifecycle Management**  
- Move old data to **Glacier** or delete unused files.  
- Enable **object versioning** for recovery from accidental deletions.  

## Storage Solution for Large E-Commerce Website Assets

### Recommend a storage solution to address the performance impact of storing large assets for an e-commerce website running on your infrastructure. Consider factors such as scalability, performance, cost, and ease of management in your recommendation.

**Answer:**

For an e-commerce website handling large assets (e.g., images, videos, product catalogs), **Amazon S3 with CloudFront** is an ideal solution due to:  

✅ **Scalability** – S3 can store unlimited data with automatic scaling.  
✅ **Performance** – CloudFront (CDN) caches frequently accessed assets at edge locations for faster delivery.  
✅ **Cost-Effectiveness** – S3 offers tiered storage, reducing costs for infrequently accessed data.  
✅ **Ease of Management** – S3 integrates with AWS services like Lambda for automation (e.g., image resizing).  

## Security Strategy for Object Storage

### Develop a comprehensive security strategy for storing 30 internal videos in object storage, addressing encryption, access controls, and monitoring measures to protect the videos from unauthorized access and ensure data security

**Answer:**

🔐 **Encryption:**  
- **Server-Side Encryption (SSE-KMS):** Encrypt videos using AWS KMS-managed keys.  
- **Client-Side Encryption:** Encrypt videos before uploading using **AWS SDKs**.  

🔒 **Access Controls:**  
- **IAM Roles & Bucket Policies:** Grant access only to authorized users (least privilege).  
- **Pre-Signed URLs:** Limit access to videos for temporary viewing.  
- **Private S3 Bucket:** Block public access completely.  

📊 **Monitoring & Logging:**  
- **AWS CloudTrail:** Track API requests to detect unauthorized access.  
- **S3 Access Logs:** Log all read/write operations.  
- **AWS Config & GuardDuty:** Continuously monitor for security risks.  


## Disaster Recovery Planning:

### Develop a disaster recovery plan for a cloud-based application, outlining strategies for data backup, redundancy, failover, and recovery procedures in the event of a catastrophic failure or natural disaster. 

**Answer:**

1️⃣ **Data Backup Strategy**  
   - Use **S3 with versioning** for application data.  
   - Use **Amazon RDS automated backups** for databases.  
   - Enable **cross-region replication** for critical data.  

2️⃣ **Redundancy & High Availability**  
   - Deploy across **multiple AWS Availability Zones**.  
   - Use **Auto Scaling** and **Load Balancers** for failover.  

3️⃣ **Failover Strategy**  
   - Set up a **standby region** (pilot-light or warm-standby setup).  
   - Use **Route 53 DNS failover** to redirect traffic to a healthy region.  

4️⃣ **Recovery Procedures**  
   - Define **RTO (Recovery Time Objective)** and **RPO (Recovery Point Objective)** based on business needs.  
   - Automate recovery with **Infrastructure as Code (IaC)** (Terraform, CloudFormation).  

5️⃣ **Testing & Drills**  
   - Conduct **regular DR tests** to validate recovery processes.  
   - Automate failover drills using **AWS Resilience Hub**.  


