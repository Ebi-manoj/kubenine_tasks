

### 1. What is the difference between a task definition and a task in ECS?

A task definition is a blueprint that describes how a container should run, including image, CPU, memory, and ports. A task is the actual running instance created from that definition.
### 2. Why does Fargate remove the need to manage EC2 instances?

It removes the need to manage EC2 because AWS handles the underlying servers, scaling, and OS maintenance. We only define the container requirements and AWS runs it for us.
### 3. What networking mode does Fargate use, and what does it mean for each task?
Fargate uses awsvpc networking mode, which gives each task its own network interface and private IP. This means each task behaves like an independent resource inside the VPC.
### 4. Why must you enable public IP assignment when running a task in a public subnet?

We must enable public IP in a public subnet because without it, the task only has a private IP and cannot be accessed from the internet
### 5. What happens if the security group does not allow inbound traffic on port 80?

The requests from the internet will be blocked. The container may be running, but it will not be reachable.
### 6. How does ECS know which container image to pull and which port to expose?

It use the information from the task definition. It reads the configuration, pulls the specified image, and runs the container with the defined port
### 7. What is a Fargate capacity provider and how does it differ from an EC2 capacity provider?

A Fargate capacity provider uses AWS-managed compute to run tasks without servers. An EC2 capacity provider uses our own EC2 instances, so we are responsible for managing and scaling them.