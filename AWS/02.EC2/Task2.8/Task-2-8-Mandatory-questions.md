
### 1. Why must an instance be stopped before changing its type?

When we stop the instance , then only the aws can reallocate a different host machine with required specification. AWS can hot swap the instance while its running.

###  2. What is the difference between `gp2`, `gp3`, and `io1` EBS volume types? When would you use each?

- gp2: Older general-purpose SSD, performance tied to size.
- gp3: Newer, cheaper, allows independent control of IOPS and throughput (preferred).
- io1: High-performance, provisioned IOPS for critical workloads like databases.  

###  3. After expanding an EBS volume in AWS, why does the OS still show the old size?

Increasing EBS size only expands the disk, not the partition or filesystem. The OS still sees the old size until you expand the partition  and filesystem.

###  4. What is a snapshot and how does it relate to creating a custom AMI?

A snapshot is a incremental backup of an EBS volume stored in S3. An AMI can also be created from the snapshot , so we will get a pre-baked configuration for the instance to launch.
### 5. Why are custom AMIs used in production environments?

Custom AMIs allow pre-installed software, configurations, and security patches. This ensures consistent, fast, and repeatable instance launches. 

### 6. What is the instance metadata service and what endpoint does it live on?

IMDS is a local service inside EC2 that provides instance information and IAM credentials. It is accessible via:
http://169.254.169.254

### 7. What is the difference between IMDSv1 and IMDSv2, and why does it matter?

IMDSv1 allows direct access with just a simple get request without any authentication . It is very vulnerable to SSRF attacks.
IMDSv2 require a token based authentication. It is more secure than IMDSv2 and recommended for production.

### 8. What is the difference between a system status check failure and an instance status check failure? What do you do for each?

System check failure shows AWS infrastructure issue .We can fix by stopping and starting instance.
Instance check failure shows OS or configuration issue. We can fix by rebooting or troubleshooting inside the instance.  
It helps identify whether the problem is AWS-side or user-side.

### 9. If you lose your PEM file and SSM is not configured, what is the volume detach/reattach recovery method?

Stop the instance, detach the root EBS volume, and attach it to another EC2 instance. Mount the volume and update the authorized keys file with a new public key. Reattach the volume to the original instance and start it to regain access.