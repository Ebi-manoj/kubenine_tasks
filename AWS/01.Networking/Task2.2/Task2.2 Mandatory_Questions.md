
## **1. What makes a subnet public?**

A subnet is public when its route table contains a default route (`0.0.0.0/0`) pointing to an Internet Gateway. This allows resources in the subnet to send traffic directly to the internet. If those resources also have a public IP, they can receive inbound traffic. The routing configuration is what defines a public subnet, not its name.

## **2. What makes a subnet private?**

A subnet is private when it does not have a direct route to the Internet Gateway. Resources inside it cannot be reached directly from the internet. If outbound internet access is needed, the subnet uses a NAT Gateway instead. This keeps the resources isolated while still allowing controlled outbound communication.

## **3. Why do we use a NAT Gateway?**

A NAT Gateway allows resources in a private subnet to access the internet for outbound requests. It performs address translation using an Elastic IP and forwards traffic through the Internet Gateway. Importantly, it blocks unsolicited inbound traffic from the internet. This ensures secure outbound-only internet access for private workloads.


## **4. Can a private subnet receive traffic directly from the internet?**

No, a private subnet cannot receive direct inbound traffic from the internet. It does not have a route to the Internet Gateway, and its resources typically do not have public IPs. NAT Gateway also does not allow inbound connections initiated from the internet. Access is only possible indirectly through components like a Load Balancer or Bastion host.


## **5. What would happen if the private subnet route pointed directly to the Internet Gateway?**

If the private subnet route table had `0.0.0.0/0 → Internet Gateway`, it would behave like a public subnet. Resources in that subnet could directly communicate with the internet. This removes the isolation and increases exposure to security risks. Essentially, it defeats the purpose of having a private subnet.


## **6. Why is this architecture considered production-ready?**

This architecture separates public-facing components from internal workloads, reducing the attack surface. Public subnets handle internet access points like NAT or Load Balancers, while private subnets host application and database layers. NAT provides controlled outbound internet access without exposing private resources. This layered design aligns with best practices for security, scalability, and maintainability.