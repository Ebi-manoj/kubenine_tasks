
### 1. What is a Launch Template and why is it required for an ASG?

A launch template is a blue print which defines the configuration like which AMI should use , which instance type , which security group etc. for to launch an instance. ASG requires this because it need information about how and what types of instance should it create.

### 2. What do minimum, desired, and maximum capacity mean? What happens if desired is set below minimum?

Minimum capacity is the lowest number of instances the ASG must always keep running, desired capacity is the number it tries to maintain, and maximum capacity is the highest number it can scale up to. AWS requires the values to follow `Minimum ≤ Desired ≤ Maximum`, so if desired is set below minimum, AWS automatically adjusts desired to match the minimum
### 3. Why should health check type be set to ELB instead of EC2 when an ASG is behind a load balancer?

ELB health checks are better because they verify whether the application is actually responding on the required port and path. EC2 health checks only confirm that the instance is running, even if the web server or application has crashed.
### 4. What happens when ASG detects an unhealthy instance? Walk through the full replacement cycle.

When the ASG detects an unhealthy instance, it first removes it from service and terminates it. Then it launches a new EC2 instance using the Launch Template, registers it with the Target Group, waits for it to pass health checks, and finally starts routing traffic to it again
### 5. Why is Multi-AZ deployment important for an Auto Scaling Group?

Multi-AZ deployment is important because it protects the application if one Availability Zone fails. By spreading instances across different AZs, the ASG can still keep the application running even if an entire data center becomes unavailable
### 6. What is the relationship between a Target Group and an ASG?

**Without a Target Group**:
- The ASG can still launch EC2 instances
- But the ALB has no way to know which instances should receive traffic
**Without an ASG**:
- You can manually register EC2 instances into a Target Group
- But you must manually replace failed instances yourself
**Together**:
- ASG manages the lifecycle of instances
- Target Group manages traffic routing and health checks
- ALB uses the Target Group to decide where to send traffic

- A Target Group acts as the connection point between the load balancer and the EC2 instances managed by the ASG. When the ASG launches or replaces instances, AWS automatically registers them with the Target Group so the load balancer can send traffic only to healthy instances.
### 7. If you manually terminate an ASG-managed instance, what does the ASG do and why?

The ASG notices that the running instance count has dropped below the desired capacity. It immediately launches a replacement instance so that the group returns to the correct number of running instances.