

### 1. What is the difference between an internet-facing ALB and an internal ALB?

An internet-facing ALB is accessible from the public internet and is used to handle external user traffic, like from browsers or mobile apps. An internal ALB, on the other hand, is only accessible within the VPC and is used for private communication between services. The main difference is **public vs private accessibility**.

---

### 2. Why would you use an internal ALB in production? Give a real example.

An internal ALB is used to keep backend services private and secure, so they are not exposed to the internet. For example, a frontend app might use an internet-facing ALB, but all backend APIs like authentication or payment services are routed through an internal ALB. This ensures only trusted services inside the VPC can access them.

---

### 3. What is path-based routing? How does the ALB decide which target group to use?

Path-based routing means the ALB forwards requests to different target groups based on the URL path, like `/app1` or `/app2`. The ALB checks the incoming request path against its listener rules and matches it with the correct condition. Once a match is found, it forwards the request to the corresponding target group.

---

### 4. What is a listener rule? How is it different from the default rule?

A listener rule defines how the ALB should route traffic based on conditions like path or host. It includes both a condition (like `/app1*`) and an action (forward to a target group). The default rule is a fallback that is used only when no other rules match, usually returning a 404 or sending traffic to a default service.

---

### 5. Why do we need separate target groups for each application?

Separate target groups allow each application to be managed independently in terms of scaling, health checks, and configuration. For example, one app can scale up without affecting another, and each can have its own health check path. This also improves isolation and makes debugging easier.

---

### 6. How do you test an internal ALB if it has no public DNS?

Since an internal ALB is not accessible from the internet, you need to test it from within the VPC. Typically, you launch a temporary EC2 instance in a public subnet, SSH into it, and use `curl` to hit the ALB’s DNS. This way, you simulate internal traffic.

---

### 7. What happens if a target in one target group becomes unhealthy — does the entire ALB go down?

No, the ALB itself does not go down if one target becomes unhealthy. It simply stops sending traffic to that unhealthy target and continues routing to healthy ones. Other target groups and services remain unaffected, ensuring high availability.