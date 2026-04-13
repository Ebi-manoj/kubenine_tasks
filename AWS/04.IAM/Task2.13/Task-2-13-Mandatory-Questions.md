

### 1. What is the difference between an IAM user and an IAM role?

An IAM user is a permanent identity with long-term credentials, usually used by humans or external apps. An IAM role, on the other hand, is not a fixed identity—it’s assumed temporarily and provides short-lived credentials. Roles are mainly used by AWS services like Amazon EC2 to access other resources securely.
### 2. What is a trust policy and what does it control?

A trust policy defines who is allowed to assume a role, such as EC2 or another AWS service. It acts like a gatekeeper—if the entity isn’t trusted here, it cannot use the role at all. Without a correct trust policy, the role is essentially unusable.

### 3. What is an instance profile and why is it needed?

An instance profile is a container that holds an IAM role and allows it to be attached to an EC2 instance. Since EC2 cannot directly use a role, the instance profile acts as a bridge between them. It enables the instance to assume the role and get credentials.

### 4. How does an EC2 instance receive temporary credentials?

When a role is attached, EC2 automatically requests temporary credentials from AWS STS behind the scenes. These credentials are then made available through the instance metadata service. The AWS SDK or CLI uses them automatically without any manual setup.

### 5. Why are long-lived access keys dangerous on EC2 instances?

Long-lived access keys don’t expire on their own, so if they are leaked, an attacker can use them for a long time. Storing them on EC2 increases the risk of exposure through files, logs, or breaches. This makes them a major security vulnerability in production.

### 6. What happens to S3 access if the IAM role is detached from the instance?

If the IAM role is detached from the instance, it immediately loses access to AWS services like Amazon S3. Since no credentials are available anymore, all requests will fail with access denied errors. This shows how access is fully controlled by the role.
### 7. What is the difference between a managed policy and an inline policy? When would you use each?

A managed policy is reusable and can be attached to multiple users or roles, making it easier to manage permissions at scale. An inline policy is directly attached to a single role or user and is tightly coupled to it. You use managed policies for general use and inline policies for very specific, one-off permissions.
### 8. Why is scoping a policy to a specific bucket ARN better than using `AmazonS3ReadOnlyAccess` in production?

Scoping a policy to a specific bucket ARN ensures the role can only access the resources it actually needs. Using broad policies like `AmazonS3ReadOnlyAccess` gives access to all buckets, which violates the principle of least privilege. In production, restricting access reduces the impact of potential security breaches.