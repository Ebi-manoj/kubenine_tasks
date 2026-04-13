
# 💾 Storage Types: Block, File, Object 

---

## 🔹 1. Block Storage

**Definition:**  
Stores data as fixed-size blocks with no structure.

### Key Points:

- Raw storage (low-level)
- OS manages data using filesystem
- High performance, low latency

### Example:

- SSD, HDD (Laptop storage)
- AWS EBS
### Flow:

```text
Block Storage → File System → Files
```

---

## 🔹 2. File Storage

**Definition:**  
Stores data as files and folders in a hierarchical structure.

### Key Points:

- Built on top of block storage
- Easy to use and manage
- Supports shared access

### Example:

- Windows (NTFS), Linux (ext4)
- AWS EFS

---

## 🔹 3. Object Storage

**Definition:**  
Stores data as objects (data + metadata + unique ID).

### Key Points:

- Flat structure (no real folders)
- Access via HTTP/API
- Highly scalable
##### Example:
- AWS S3

---
# 💾 Block Storage ↔ File System 

---

## 🔹 1. Core Idea

- **Block Storage** = raw disk (unstructured data)
- **File System** = layer that organizes this raw data into usable files

👉 Relationship:

```text
Application → File System → Block Layer → Disk
```

---

## 🔹 2. What is Block Storage?

**Definition:**  
Stores data in fixed-size blocks (no structure, no meaning)

### Key Points:

- Data stored as raw bytes
- Each block has an address
- No file names or folders

##### Example:

- SSD / HDD (Laptop)
- AWS EBS

---

## 🔹 3. Problem Without File System

Without filesystem:

- No file names ❌
- No structure ❌
- No metadata ❌
- Cannot retrieve data meaningfully ❌

👉 Data exists but is **unusable**

---

## 🔹 4. What is a File System?

**Definition:**  
A system that organizes blocks into files and directories using metadata.

### Responsibilities:

- File naming
- Directory structure
- Block allocation
- Metadata management

---

## 🔹 5. Internal Working of File System

### Key Components:

#### 1. Superblock

- Stores filesystem info (size, layout)
#### 2. Inode (Linux - ext4)

- Represents a file
- Stores:
   - File size  
   - Permissions
   - Block pointers
 

#### 3. Data Blocks
- Actual file content

---

## 🔹 6. How File is Stored (Write Flow)

Example: Saving `test.txt`

```text
1. Application sends data
2. File system:
   → Finds free blocks
   → Creates inode
   → Maps file → blocks
3. Block layer writes to disk
```

### Representation:

```text
test.txt
   ↓
inode #45
   ↓
Blocks → [10, 11, 15]
```

---

## 🔹 7. How File is Read

```text
1. Locate file → inode
2. Inode gives block numbers
3. Read blocks
4. Reconstruct file
```

---

## 🔹 8. Linux Practical Flow

### 1. Check block devices:

```bash
lsblk
```

### 2. Raw disk (no FS):

```bash
file -s /dev/xvdb
```

### 3. Create filesystem:

```bash
mkfs.ext4 /dev/xvdb
```

### 4. Mount:

```bash
mount /dev/xvdb /mnt/mydisk
```

### 5. Use files:

```bash
echo "data" > /mnt/mydisk/file.txt
```

---

## 🔹 9. What is Mounting?

**Mount = attaching filesystem to OS directory**

```text
/dev/xvdb → /mnt/mydisk
```

👉 Makes disk accessible as files

---

## 🔹 10. Types of File Systems

|Type|Description|
|---|---|
|ext4|Linux filesystem (inode-based)|
|NTFS|Windows filesystem (MFT-based)|
|NFS|Network filesystem|

---
# 💽 EBS Volume Types & Storage Concepts 

---

## 🔹 1. gp2 vs gp3 vs io1

### ✅ gp2 (General Purpose SSD – Old)

- Performance tied to **volume size**
- Baseline: **3 IOPS per GB**
- Burst capability
- ❗ Inefficient → need bigger size for more performance

---

### ✅ gp3 (General Purpose SSD – Recommended)

- Performance **independent of size**
- Default:
   - 3000 IOPS
   - 125 MiB/s throughput
- Scalable:
- up to 16,000 IOPS
- 💰 Cheaper than gp2

---

### ✅ io1 (Provisioned IOPS SSD)

- **User-defined IOPS**
- High performance & low latency
- Consistent (no burst)
- 💸 Expensive

---

## 🔹 2. When to Choose Each

|Workload|Volume Type|
|---|---|
|General apps, web servers|gp3|
|Dev/Test|gp3|
|Legacy systems|gp2|
|Databases, critical apps|io1|

---

## 🔹 3. Why gp3 is Preferred over gp2

- Performance not tied to size
- More cost-efficient
- Better control over IOPS & throughput

👉 **gp3 = default choice for new workloads**

---

## 🔹 4. EBS-backed vs Instance Store

### ✅ EBS-backed

- Network-attached storage
- Persistent (data survives stop/start)
- Flexible (detach/attach, snapshots)
- Used for:
   -  OS
   - Databases
   - Production systems

---

### ⚡ Instance Store

- Physically attached to host
- Very high performance
- ❌ Ephemeral (data lost on stop/terminate)

Used for:
- Cache
- Temporary data
- Buffers

---
# 🌐 EC2 Instance Metadata Service (IMDS) – Concise Notes

---

## 🔹 1. What is IMDS?

- **Instance Metadata Service (IMDS)** is a **local HTTP service inside an EC2 instance**
- Provides **information about the instance** and **temporary IAM credentials**
- Used by applications and scripts running inside the instance

---

## 🔹 2. Endpoint

```text
http://169.254.169.254
```

- Link-local IP (only accessible from inside the instance)
- Not reachable from the internet

---

## 🔹 3. What information is available?

### 🔍 Instance Metadata:

- Instance ID
- Instance type
- Public IP / Private IP
- Availability Zone
- Region (derived from AZ)

---

### 🔐 IAM Role Credentials:

- Temporary Access Key
- Secret Key
- Session Token

👉 Used for secure AWS API access without hardcoding credentials

---

## 🔹 4. Why IMDS matters

### ✅ Automation

- Scripts auto-detect environment (region, instance ID)

---

### ✅ Dynamic Configuration

- Same app adapts based on:
  - Region
  - Instance identity


---

### ✅ Secure AWS Access

- Uses IAM roles instead of static credentials
- No secrets stored in code or environment variables

---

### ✅ Scalable Systems

- Instances self-register (autoscaling, load balancers)

---

## 🔹 5. IMDSv1 vs IMDSv2

### ❌ IMDSv1

- Simple HTTP requests (no authentication)
- Vulnerable to SSRF attacks

---

### 🔐 IMDSv2

- Requires **session token**
- Two-step process:
   1. Get token (HTTP PUT)
   2. Use token in request header

---
# 🖥️ EC2 Status Checks 

---

## 🔹 1. What are Status Checks?

- AWS monitors EC2 health using status checks
- Helps identify whether issue is:
   - AWS infrastructure
   - Your instance (OS/config)

---

## 🔹 2. Types of Status Checks

### ✅ System Status Check (AWS Side)

- Checks:
   - Hardware
   - Network
   - Power
- ❌ Failure means:
   - AWS infrastructure issue

- 🔧 Action:
   - Stop → Start instance (moves to new host)

---

### ✅ Instance Status Check (Your Side)

 **Checks**:
- OS health
- Network inside instance
- Boot process

 ❌ Failure means:
  - OS/config issue
  
- 🔧 Actions:
  - Reboot instance
  - Check logs
  - Fix configs
  - Use rescue (detach volume)
