### 1. What is a Load Balancer and why do we need one?

 load balancer distributes incoming traffic across multiple servers so no single server gets overloaded. This improves performance, reliability, and ensures our application stays available even if one server fails
### 2. What is a Target Group? How does it relate to the ALB?

A target group is a collection of backend resources like EC2 instances that receive traffic from the ALB. The ALB doesn’t send requests directly to instances, it forwards them to the target group, which then routes to healthy instances
### 3. What is a Listener and what does it do?

Its a process that checks for incoming requests on a specific port and protocol, like HTTP on port 80. It then decides where to send that traffic
### 4. Why should EC2 instances not be publicly accessible in production?

Keeping EC2 instances private reduces the attack surface and protects them from direct internet access. Instead, all traffic should go through controlled entry points like a load balancer
### 5. What happens if one of the two EC2 instances fails the health check?

If an instance fails health checks, the ALB marks it as unhealthy and stops sending traffic to it. All requests are then routed only to the remaining healthy instance.
### 6. Why must the ALB be in public subnets?

The ALB needs to receive traffic from the internet, which is only possible through public subnets connected to an Internet Gateway
### 7. What layer does ALB operate on? What does that mean?

It operates at Layer 7, the application layer, which means it understands HTTP/HTTPS requests. This allows it to make smart routing decisions based on URLs, headers, or paths
### 8. The EC2 security group allows HTTP only from the ALB security group. Why is this more secure than allowing from `0.0.0.0/0`?

Allowing traffic only from the ALB security group ensures that only the load balancer can access the EC2 instances. This prevents direct access from the internet