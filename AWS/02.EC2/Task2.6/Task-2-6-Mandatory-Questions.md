
### 1. What is an AMI and why does your choice of AMI matter?

An AMI is a pre-configured template that defines the OS, software, and settings for an EC2 instance. Your choice matters because it determines the environment, compatibility, performance, and tools available on the instance.

### 2. What is the difference between stopping and terminating an EC2 instance?

Stopping an instance shuts it down but preserves its data and configuration, allowing it to be restarted later. Terminating permanently deletes the instance and, by default, its associated root volume.
### 3. Why does the public IP change after you stop and start an instance?

When you stop an instance, AWS releases its public IP back to the pool. On starting again, a new public IP is assigned because the instance may be launched on a different host.
### 4. What happens to the root EBS volume when you terminate an instance with "Delete on Termination" enabled?

If "Delete on Termination" is enabled, the root EBS volume is automatically deleted when the instance is terminated. This ensures no leftover storage or data remains unless explicitly preserved.
### 5. Why should SSH access never be open to `0.0.0.0/0` in production?

Allowing SSH open to 0.0.0.0/0 will increase the attack surface , because anyone can try to login to our ec2 instance , even though it is very hard to login for others, the main intention for them is to flood the requests to our instance.
### 6. What would happen if your security group had no inbound rules at all?

A security group with no inbound rules blocks all incoming traffic by default. This means no one can access the instance, including SSH,  making it unreachable.
### 7. You created a VPC, subnet, IGW, and route table just to launch one instance — why are all four needed?

These components together enable network connectivity, VPC provides isolation, subnet defines the network segment, IGW enables internet access, and the route table directs traffic. Without any one of them, the instance cannot properly communicate with external networks