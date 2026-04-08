
### 1. Why is CIDR planning important before creating a VPC?

CIDR planning is important , it ensures you have a required IPs available in current architecture and if any scaling also going to happen in the future . So we should plan accordingly that it doesn't goes to a situation like IP exhaustion.

### 2. Why should production workloads live in private subnets?

Those resources are important and we should add much as possible security . In this case we are preventing direct exposure to the internet and there by we can reduce the attack surface.
### 3. Why must NAT Gateway exist in a public subnet?

Nat gateway need a access to the internet gateway so that it can reach the internet . We only attached a route table that points to internet gateway is in public subnet . So we should place NAT in public subnet then only it can reach the internet gateway.
### 4. Why are Security Groups preferred over NACL in most cases?

Security Groups are stateful, meaning return traffic is automatically allowed, making them easier to manage.
### 5. How does Multi-AZ improve resilience?

Multi-AZ distributes resources across physically separate data centers, reducing the impact of a single failure. If one AZ goes down, traffic can be routed to another AZ without downtime. This ensures high availability and fault tolerance.

### 6. What is the biggest risk in your current VPC design?

The  mistake is that i have placed an ec2 instance in public subnet , but this is for a demo . Later we keep it in private subnet and distribute the traffic using a ALB.