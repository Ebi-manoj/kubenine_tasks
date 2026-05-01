
### 1. What is the difference between the ECS Task Execution Role and the Task Role? Give a concrete example of what each one does.

The execution role is used by ECS itself to pull images from ECR and send logs to CloudWatch. The task role is used by our application code inside the container, for example when our app reads files from S3.
### 2. Your task role grants `s3:GetObject` on a specific bucket. Why is this better than attaching `AmazonS3ReadOnlyAccess`?

Granting only s3:GetObject to a specific bucket follows least privilege, so our app can only access exactly what it needs. AmazonS3ReadOnlyAccess would allow reading all buckets, which increases the blast radius if compromised.
### 3. Why do ECS Fargate tasks in private subnets need a NAT Gateway? What specific things need internet access?

Tasks in private subnets need internet access to pull container images from ECR and send logs to CloudWatch. Without NAT, they cannot reach these AWS public endpoints.
### 4. The ECS security group allows port 8501 only from the ALB security group. Why not from `0.0.0.0/0`?

Allowing traffic only from the ALB ensures all requests go through a controlled entry point. Opening it to 0.0.0.0/0 would expose the service directly to the internet, bypassing load balancing and security layers.
### 5. What is the ALB target group type for Fargate and why is it `ip` instead of `instance`?

Fargate uses ip target type because tasks don’t run on fixed EC2 instances. Each task gets its own ENI and IP, so the ALB routes traffic directly to those IPs.
### 6. How does CloudWatch know the CPU/memory utilization of your ECS service? Where do these metrics come from?

CloudWatch gets CPU and memory metrics from ECS, which collects them via the container runtime and underlying infrastructure. These are automatically pushed as service-level metrics.
### 7. If the Streamlit container crashes, what happens? Does ECS restart it? How would you find out what went wrong?

ECS will automatically restart the task to maintain the desired count. We can check CloudWatch logs and ECS task events to understand why it failed.
### 8. Compare this ECS deployment to the EC2 deployment in Task 3.44. What are the pros and cons of each approach?

ECS with Fargate removes server management and scales easily, but gives less control over infrastructure. EC2 gives more control and can be cheaper at scale, but requires managing instances, patching, and capacity planning.