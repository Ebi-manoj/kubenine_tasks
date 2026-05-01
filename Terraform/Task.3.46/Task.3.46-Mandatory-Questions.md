

### 1. Why do we place the ALB in public subnets and EC2 instances in private subnets?

We place the ALB in public subnets so it can receive traffic directly from the internet, while keeping EC2 instances in private subnets to avoid exposing them publicly. This way, all external access is controlled through the load balancer, improving security.
### 2. The EC2 security group allows HTTP only from the ALB security group. Why is this more secure than allowing HTTP from `0.0.0.0/0`?

Allowing HTTP only from the ALB security group ensures that traffic reaches EC2 instances only through the load balancer, not directly from the internet. If we allowed 0.0.0.0/0, anyone could bypass the ALB and hit the instances directly, which weakens the security model.
### 3. What is a target group? How does the ALB know where to send traffic?

A target group is a logical group of backend resources, like EC2 instances, that the ALB routes traffic to. The ALB uses the target group configuration and health checks to decide which instances should receive incoming requests.
### 4. What happens if one of the two EC2 instances fails the health check? Does the site go down?

If one EC2 instance fails the health check, the ALB automatically stops sending traffic to that instance and continues routing to the healthy one. So our site stays up as long as at least one instance is healthy.
### 5. Why does the ALB require subnets in at least 2 Availability Zones?

 The ALB requires subnets in at least two Availability Zones to ensure high availability and fault tolerance. If one AZ goes down, the ALB can still handle traffic from another AZ without interruption.
### 6. The EC2 instances are in private subnets with no public IP. How did `user_data` install Nginx? What made that possible?

Even though the EC2 instances are in private subnets, they can access the internet through the NAT Gateway. This allows user_data to download and install Nginx during instance initialization.
### 7. Explain your VPC CIDR design. Why did you choose those specific CIDR ranges?

We chose a /16 CIDR block for the VPC to give enough IP space for scaling, and split it into smaller /24 subnets for public and private layers across multiple AZs. This keeps the network organized and avoids IP conflicts as the infrastructure grows.
### 8. If you wanted to add HTTPS (port 443) to this setup, what would you need to change?

 To add HTTPS, we would need to attach an SSL certificate (from ACM) to the ALB and create a listener on port 443. We would also update the security group to allow HTTPS traffic and optionally redirect HTTP to HTTPS.