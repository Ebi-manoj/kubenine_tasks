
### 1. What is the difference between a Task Role and a Task Execution Role? Why are they separate?

The Task Execution Role is used by ECS itself to pull images, send logs to CloudWatch, and fetch secrets during startup. The Task Role is used by our application code inside the container to access AWS services like S3 or SSM.
### 2. Why should ECS tasks run in private subnets instead of public subnets?

Running tasks in private subnets prevents direct exposure to the internet, reducing attack surface. Traffic is controlled through the ALB, making the setup more secure
### 3. Why do private subnets need a NAT Gateway for ECS Fargate tasks to work?

Even though tasks are private, they still need outbound internet access to pull images and call AWS APIs. The NAT Gateway allows this outbound traffic without exposing the tasks publicly
### 4. Why must the ALB target type be IP when using Fargate?

Fargate tasks don’t run on EC2 instances, so there’s no instance ID to register. Instead, each task gets its own ENI and private IP, so the ALB routes traffic using IP-based targets
### 5. What happens if the container exceeds its configured memory limit?

If a container uses more memory than allocated, Fargate will terminate it with an out-of-memory (OOM) kill. The ECS service may then restart the task to maintain the desired count.
### 6. Why is it a security risk to give the Task Role broad managed policies like `AmazonS3FullAccess`?

Giving full access violates the principle of least privilege and increases the risk if the container is compromised. A limited policy ensures the app can only access what it truly needs.
### 7. How does the ALB know which task IPs to send traffic to?

ECS automatically registers and deregisters task IPs with the target group. The ALB then uses health checks to decide which of those IPs are healthy and can receive traffic.
### 8. What is the difference between task-level and container-level resource allocation in Fargate?

In Fargate, CPU and memory are defined at the task level, and all containers share those resources. Unlike EC2, we don’t assign hard limits per container unless we configure them explicitly.