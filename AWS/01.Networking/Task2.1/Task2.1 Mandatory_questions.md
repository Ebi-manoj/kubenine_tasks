
## **1. What is Public Cloud?**

Public cloud is a model where a cloud provider like AWS offers computing resources such as servers, storage, and networking over the internet. These resources are shared physically but isolated logically between users. It allows on-demand access and scaling without managing physical infrastructure. Users pay only for what they use.

---

## **2. What is a VPC?**

A VPC (Virtual Private Cloud) is a logically isolated network within AWS where you can launch and manage resources. It allows you to define IP ranges, subnets, routing, and internet access. It acts like your own private data center inside AWS. All your cloud resources are deployed within this network boundary.

---

## **3. Why is CIDR planning important?**

CIDR planning defines the IP address range for your VPC and subnets. Proper planning ensures enough IP space for scaling and prevents overlapping networks. It also helps in organizing subnets and enabling future integrations like VPC peering or VPN. Poor CIDR design can lead to major networking issues later.

---

## **4. What makes a subnet "public"?**

A subnet is considered public when its route table contains a default route (`0.0.0.0/0`) pointing to an Internet Gateway. This allows resources in the subnet to communicate directly with the internet. If those resources also have public IPs, they can receive inbound traffic. The routing configuration, not the name, determines whether a subnet is public.