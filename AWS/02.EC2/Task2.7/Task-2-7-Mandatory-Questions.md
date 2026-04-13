### 1. Why can the public EC2 instance be reached from the internet but the private one cannot?

A public EC2 instance has a public IP and a route (`0.0.0.0/0`) to the Internet Gateway, enabling direct inbound and outbound communication. A private instance lacks a public IP and does not have a route from the Internet Gateway, so it cannot receive inbound internet traffic. This isolation is intentional for security.

### 2. What is User Data and when exactly does it execute during the instance lifecycle?

User Data is a bootstrap script provided at launch that is executed by **cloud-init** during the instance’s first boot cycle. It runs after the OS initializes but before the instance is fully ready for use. By default, it executes only once unless explicitly configured otherwise.


### 3. What happens if the User Data script has a bug — does the instance still launch?

Yes, the instance will still launch because User Data execution is not a blocking step. However, the intended configurations (like installing packages) may fail. Errors can be diagnosed using logs such as `/var/log/cloud-init-output.log`.


### 4. What problem does an Elastic IP solve that an auto-assigned public IP does not?

Auto-assigned public IPs change when an instance is stopped and started, breaking DNS mappings or external integrations. An Elastic IP provides a static, persistent public IP that you can remap to another instance if needed. This ensures consistent external connectivity.


### 5. How does the private instance reach the internet for software updates if it has no public IP?

The private instance routes outbound traffic to a NAT Gateway located in a public subnet. The NAT Gateway translates the private IP to its own public IP and forwards the request via the Internet Gateway. This allows outbound-only internet access without exposing the instance.


### 6. Why is the private security group configured to allow SSH only from the public security group, not from your IP?

This enforces a **bastion host pattern**, where SSH access must go through a controlled public instance. It prevents direct internet exposure of private instances, reducing attack surface. Access is tightly scoped to trusted internal sources instead of arbitrary external IPs.

### 7. What would happen if you accidentally deleted the NAT Gateway while the private instance was running?

The private instance would immediately lose outbound internet connectivity, affecting updates, API calls, or external dependencies. Inbound traffic would remain unaffected (still blocked). This creates a single point of failure if no redundant NAT is configured.

### 8. If HTTP port 80 is not open in the security group, what happens when you try to access Nginx?

The request will be blocked at the security group level, and the connection will time out or be refused. Even if Nginx is running correctly on the instance, it will not be reachable. Security groups act as the first line of access control for such traffic.