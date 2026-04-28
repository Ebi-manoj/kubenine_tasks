
## 📌 What is Terraform?

Terraform is an **Infrastructure as Code (IaC)** tool used to:

- Define cloud infrastructure using code
- Automate provisioning (no manual AWS console work)
- Manage infrastructure lifecycle (create, update, destroy)

---

## ❓ Why Terraform is Needed

### Without Terraform 

- Manual setup in AWS console
- Error-prone
- Hard to reproduce environments
- No version control

### With Terraform 

- Infrastructure = Code (HCL)
- Repeatable & consistent
- Version-controlled (Git)
- Easy to scale & automate

---

## 🧠 Core Concept

Terraform follows a **declarative approach**:

> "This is what I want" → Terraform figures out how to create it

---

## 📁 Writing a Terraform File

### Main file: `main.tf`

This is the primary file where you define:

- Provider (AWS, Azure, GCP)
- Resources (EC2, S3, etc.)

---

## 🔌 Provider Block

Represents the cloud provider.

```hcl
provider "aws" {
  region  = "ap-south-1"
  profile = "ebi-sso"
}
```

### Notes:

- Required to tell Terraform **where to create resources**
- Each provider has different configuration options
- Syntax remains consistent across providers

---

## 🧱 Resource Block

Defines infrastructure.

```hcl
resource "aws_instance" "my_ec2" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

### Pattern:

```hcl
resource "<provider_resource>" "<local_name>" {
  ...
}
```

---

## 🔤 Variables in Terraform

Used to make code dynamic and reusable.

### Supported Types:

- string
- number
- bool
- list
- map

---

## 🧾 Defining Variables (`variables.tf`)

```hcl
variable "instance_type" {
  type    = string
  default = "t2.micro"
}
```

```hcl
variable "tags" {
  type = map(string)
  default = {
    Name = "MyInstance"
  }
}
```

### Why `variables.tf`?

- Separates configuration from logic
- Cleaner code structure
- Easy reuse across environments

---

## 📦 Using Variables in `main.tf`

```hcl
resource "aws_instance" "my_ec2" {
  ami           = "ami-123456"
  instance_type = var.instance_type
  tags          = var.tags
}
```

---

## 📄 `terraform.tfvars` (Important)

Used to assign values to variables.

```hcl
instance_type = "t3.micro"

tags = {
  Name = "ProdServer"
}
```

---

### Why use `terraform.tfvars`?

- Avoid hardcoding values
- Environment-specific configs
- Keeps secrets/config separate

---

## 📁 Multiple `.tfvars` Files

Useful for different environments:

### Example:

- `dev.tfvars`
- `prod.tfvars`

Run like this:

```bash
terraform apply -var-file="dev.tfvars"
```

---

## Locals

### 🔹 Definition

`locals` are **internal variables** used to simplify expressions and avoid repetition.

### 🔹 Key Points

- Defined inside `locals {}` block
- Cannot be overridden externally
- Used for **computed values**

#### 🔹 Example


```
locals {  
  environment = "dev"  
  app_name    = "myapp"  
  full_name   = "${local.app_name}-${local.environment}"  
}
```
#### Usage


```
resource "aws_s3_bucket" "example" {  
  bucket = local.full_name  
}
```

---

####  When to Use

- Combine variables
- Avoid repeating logic
- Improve readability

---

## Count

### 🔹 Definition

`count` is used to create **multiple identical resources**

### 🔹 Key Points

- Works with **index (count.index)**
- Identity = position (0,1,2…)
- Can cause issues if list changes

#### 🔹 Example

```
variable "user_names" {  
  default = ["user1", "user2", "user3"]  
}  
  
resource "aws_iam_user" "example" {  
  count = length(var.user_names)  
  name  = var.user_names[count.index]  
}
```


#### Use Case

- Fixed number of resources
- Identical infrastructure (e.g., 3 EC2 instances)
##### ⚠️ Problem

List change → resource recreation due to index shift

---

## for_each

### 🔹 Definition

`for_each` creates multiple resources using **unique keys**

### 🔹 Key Points

- Works with **map or set**
- Identity = key (stable)
- Safer than `count`

#### 🔹 Example (List)

```
variable "users" {  
  default = ["alice", "bob", "charlie"]  
}  
  
resource "aws_iam_user" "example" {  
  for_each = toset(var.users)  
  name     = each.value  
}
```

---

#### 🔹 Example (Map)

```
variable "instances" {  
  default = {  
    dev  = "t2.micro"  
    prod = "t2.large"  
  }  
}  
  
resource "aws_instance" "example" {  
  for_each = var.instances  
  
  instance_type = each.value  
  
  tags = {  
    Name = each.key  
  }  
}
```


#### Use Case

- Named resources (IAM users, S3 buckets)
- Environment-based configs
- Anything needing stable identity

---

## Provisioners

### 🔹 Definition

Provisioners run **scripts/commands after resource creation**

### 🔹 Types

- `local-exec` → runs on your machine
- `remote-exec` → runs inside resource


### 🔹 Example (local-exec)

```
resource "aws_instance" "example" {  
  ami           = "ami-123456"  
  instance_type = "t2.micro"  
  
  provisioner "local-exec" {  
    command = "echo Instance created!"  
  }  
}
```


### 🔹 Example (remote-exec)

```
provisioner "remote-exec" {  
  inline = [  
    "sudo apt update",  
    "sudo apt install nginx -y"  
  ]  
}

```

#### ⚠️ Important

- Provisioners are **last resort**
- Not recommended for production (use user_data, Ansible, etc.)

---

## Dynamic Blocks

### 🔹 Definition

Used to **generate nested blocks dynamically**

### 🔹 Example (Security Group Rules)

```
variable "ports" {  
  default = [80, 443, 22]  
}  
  
resource "aws_security_group" "example" {  
  name = "dynamic-sg"  
  
  dynamic "ingress" {  
    for_each = var.ports  
    content {  
      from_port   = ingress.value  
      to_port     = ingress.value  
      protocol    = "tcp"  
      cidr_blocks = ["0.0.0.0/0"]  
    }  
  }  
}
```

---

### 🧠 How it works

- `dynamic "ingress"` → block name
- `for_each` → loop
- `content` → actual block content

---

### 🧠 Use Case

- Repeating nested blocks
- Security groups
- Load balancer listeners
- Complex configs

---

terraform modules
