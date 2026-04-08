### 1. What is an Availability Zone?

An Availability Zone is a physically separate data center within an AWS region, with its own power, networking, and infrastructure

### 2. Why is Single-AZ architecture risky?

Single AZ architecture has a single point of failure, so if that AZ goes down due to hardware or network issues, the entire application becomes unavailable
### 3. How does Multi-AZ improve availability?

We are distributing our resources in multiple AZ , so if one AZ goes down , others can still continue serving traffic. This avoid SPOF and increase system uptime and system reliability. 

### 4. What would happen if one AZ goes down?

If one AZ fails, all resources in that AZ become unavailable, but resources in other AZs continue running. With proper setup, traffic can be routed to healthy AZs, minimizing downtime.
### 5. Does Multi-AZ increase cost? Why?

Yes definitely, Multi-AZ increases cost because it requires duplicate resources like  NAT Gateways, and possibly instances across AZs

### 6. Why is high availability critical in production systems?

In a production systems , the system should be highly available , it is not an optional. This reduce system down time and make system reliable and a good user experience.