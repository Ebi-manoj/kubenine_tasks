
### 1. What is a Terraform module? Why would you use a module instead of writing raw resources?

A Terraform module is a reusable package of Terraform code that encapsulates a set of resources. We use modules to avoid repetition, enforce consistency, and simplify complex infrastructure instead of writing and maintaining raw resources every time.
### 2. What resources does the VPC module create behind the scenes when you call it?

The VPC module provisions core networking components like the VPC, subnets, route tables, Internet Gateway, NAT Gateway, Elastic IP, and optionally NACLs. It also handles associations between these resources, which reduces manual configuration errors.
### 3. You set `single_nat_gateway = true`. What would happen if you set it to `false`? When would you want one NAT per AZ?

If set to false, the module creates one NAT Gateway per availability zone, increasing availability but also cost. We prefer one NAT per AZ in production systems where high availability and fault tolerance are critical.
### 4. How do you pin a module to a specific version? Why is this important?

We pin a module using the version argument in the module block. This is important to ensure consistent and predictable deployments, avoiding unexpected changes from newer module updates.
### 5. How does the VPC module know which subnet is public vs private?

The module determines this based on inputs like public_subnets and private_subnets, and whether routes to the Internet Gateway or NAT Gateway are configured. Public subnets get direct IGW routes, while private ones route through NAT.
### 6. If you wanted to add a database subnet tier (3 more subnets), how would you modify the module call?

We can extend the module by adding another subnet list like `database_subnets` with corresponding CIDRs and AZs.
### 7. Compare this Terraform approach to when you built the VPC manually in Task 2.1-2.4. What's easier? What's harder? What's better for a team?

Using modules is easier and faster since it abstracts complexity and reduces boilerplate. Manual setup gives deeper control and learning, but modules are better for teams because they enforce standardization and reduce mistakes.
### 8. Why do we need a dedicated NACL for public subnets? What would happen if we only relied on Security Groups?

A dedicated NACL provides an additional layer of control at the subnet level, especially for stateless filtering of traffic.
### 9. Why do NACL rules need to allow ephemeral ports (1024-65535)?

Ephemeral ports are used for return traffic in client-server communication. If we don’t allow them, responses from servers  would be blocked, breaking normal network communication.