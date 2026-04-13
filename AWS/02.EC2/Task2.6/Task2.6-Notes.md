

# EC2 instance types
### 🔹 1. General Purpose Instances

**Balanced resources (CPU + Memory + Networking)**

- Designed for everyday workloads
- No extreme specialization
- Good starting point for most apps

**Examples:**

- t3.micro
- m5.large

**Use case:**

- Web servers
- Small backend APIs
- Dev/test environments

**👉 Why choose?**  
When you don’t have a specific bottleneck (CPU or memory), and you want a **cost-effective, balanced machine**.

---

### 🔹 2. Compute Optimized Instances

**High CPU power**

- More vCPUs relative to memory
- Ideal for compute-heavy operations

**Examples:**

- c5.large

**Use case:**

- Data processing
- Video encoding
- High-performance web servers

**👉 Why choose?**  
If your application is **CPU-bound** (e.g., heavy calculations), this gives better performance than general-purpose.

---

### 🔹 3. Memory Optimized Instances

**High RAM capacity**

- Large memory compared to CPU
- Designed for fast data access in memory

**Examples:**

- r5.large

**Use case:**

- Databases (MySQL, MongoDB)
- Caching (Redis)
- Real-time analytics

**👉 Why choose?**  
If your app stores or processes **large datasets in memory**, this prevents slow disk access.

---

### 🔹 4. Storage Optimized Instances

**High-speed disk (IOPS) performance**

- Uses NVMe SSDs or high-throughput storage
- Optimized for frequent read/write

**Examples:**

- i3.large

**Use case:**

- Big data workloads
- NoSQL databases
- Log processing systems

**👉 Why choose?**  
If your app is **disk I/O intensive**, like reading/writing data continuously.

---

### 🔹 5. Accelerated Computing Instances

**Uses GPU / specialized hardware**

- Includes GPUs or AI accelerators
- Designed for parallel processing

**Examples:**

- p3.2xlarge

**Use case:**

- Machine Learning / AI
- Image processing
- 3D rendering

**👉 Why choose?**  
If your workload needs **massive parallel computation**, GPUs are far faster than CPUs.

---

# Amazon Machine Image (AMI)

An **AMI** is a **pre-configured template** used to launch EC2 instances. It contains the **operating system, installed software, application code, and configurations**, acting as a **blueprint** for creating identical virtual servers.

### 🔑 Key Points:

- Used to **quickly launch EC2 instances**
- Ensures **consistency and repeatability**
- Types:
    - **AWS-provided** (e.g., Ubuntu, Amazon Linux)
    - **Custom AMI** (your configured setup)
    - **Marketplace AMIs** (prebuilt with tools like WordPress)

### 🚀 Importance:

- Enables **fast deployment**
- Supports **scaling (Auto Scaling Groups)**
- Avoids repeated manual setup

---
### RSA & EDA25519

RSA is a traditional cryptographic algorithm using large keys(2048bit), while ED25519 is a modern elliptic-curve algorithm that provides better performance and security with smaller keys(~256bit).

---
# Instance State

## 🔄 Reboot (Restart)

- OS-level restart only
- Same host, same infrastructure
- Public IP → ✅ same
- Private IP → ✅ same
- EBS → unchanged
- RAM → cleared

👉 **Use when:** system issues, applying updates

---

## 🛑 Stop

- Instance is powered off (like shutdown)
- Underlying host may change later
- Public IP → ❌ lost
- Private IP → ✅ same
- EBS → ✅ persists
- RAM → ❌ lost
- Billing → stops (compute only)

👉 **Key idea:** compute stops, storage remains

---

## ▶️ Start

- Instance boots again
- May run on a new physical host
- Public IP → ✅ new assigned
- Private IP → ✅ same
- EBS → reattached
- RAM → fresh

👉 **Key idea:** same disk, new runtime

---

## ❌ Terminate

- Instance permanently deleted
- Public & private IP → ❌ gone
- EBS (root volume) → ❌ deleted (default)
- Cannot recover

👉 If **Delete on Termination = false** → EBS survives

---

## 💾 Root Volume (EBS)

Amazon EBS

- Main disk (`/dev/sda1`)
- Contains OS, apps, files
- Stored **outside EC2** in EBS
- Attached to instance

---
