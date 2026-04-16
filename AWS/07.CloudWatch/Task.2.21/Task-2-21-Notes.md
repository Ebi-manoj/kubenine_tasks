
# CloudWatch Fundamentals

## 1. What is CloudWatch

**Amazon CloudWatch** is AWS’s monitoring and observability service.  
It collects, stores, and analyzes system data (metrics and logs) and enables alerting and automation.

**Role in AWS:**

- Monitor resource health (EC2, RDS, etc.)
- Visualize performance (graphs, dashboards)
- Trigger alarms and automated actions (e.g., Auto Scaling)

---

## 2. Metrics vs Logs

**Metrics**

- Numerical data over time (e.g., CPUUtilization = 70%)
- Lightweight and fast
- Used for monitoring, alerting, and scaling

**Logs**

- Detailed text records (e.g., error messages)
- Heavy and verbose
- Used for debugging and root cause analysis

**Key Difference:**

- Metrics → _What is happening_
- Logs → _Why it is happening_

---

## 3. Namespace

A **namespace** is a logical container for metrics.

**Purpose:**

- Organizes metrics by service or application
- Avoids naming conflicts

**Examples:**

- `AWS/EC2` → default EC2 metrics
- `CWAgent` → metrics from CloudWatch Agent
- `MyApp/Metrics` → custom application metrics

**Metric Identity = Namespace + Metric Name + Dimensions**

---

## 4. Default vs Custom Metrics

**Default Metrics**

- Provided automatically by AWS
- Examples: CPUUtilization, NetworkIn, DiskReadOps
- Source: hypervisor level (outside OS)
- No setup required

**Custom Metrics**

- User-defined metrics sent to CloudWatch
- Examples: memory usage, disk usage, app-level metrics
- Source: inside OS or application
- Requires CloudWatch Agent or SDK

**Key Insight:**

- Default → infrastructure visibility
- Custom → application/OS visibility

---

# 📊 3. Hypervisor-Level Metrics (Default Metrics)

These are metrics AWS can see **without entering your OS**.

---

## ✅ Why CPU is Available

CPU usage is tracked at the hypervisor:

- Hypervisor allocates CPU time to your instance
- It knows:
    - How much CPU you are consuming
    - When your instance is idle or busy

👉 So AWS can expose:

- `CPUUtilization`

---

## ✅ Why Network Metrics are Available

Network traffic flows through AWS-controlled infrastructure:

- All packets pass through AWS networking layer
- Hypervisor tracks:
    - Incoming bytes
    - Outgoing bytes

👉 So AWS provides:

- `NetworkIn`
- `NetworkOut`

---

## ✅ Why Disk I/O is Available

Disk operations go through virtualized storage:

- Reads/writes are handled via AWS storage layer (EBS)
- Hypervisor can observe:
    - Read operations
    - Write operations

👉 So AWS provides:

- `DiskReadOps`
- `DiskWriteOps`

---
# 🚫 4. OS-Level Metrics (Not Available by Default)

These exist **inside your EC2 instance**.

---

## ❌ Memory Usage

- RAM is allocated to your VM
- Inside OS:
    - Processes consume memory
    - Kernel manages allocation

👉 Hypervisor only knows:

- “This VM has 4GB RAM”

👉 It does NOT know:

- “2.5GB is used”

---

## ❌ Disk Usage (Storage Space)

Example command:

df -h

Shows:

- Used space
- Free space

👉 This is:

- File system level
- Managed inside OS

👉 AWS cannot see:

- Which files exist
- How full your disk is

---

## 🔒 Why AWS Cannot See This

Because of the **virtualization boundary**:

- Strong isolation between tenants
- AWS cannot inspect your OS internals
- Security + privacy guarantee