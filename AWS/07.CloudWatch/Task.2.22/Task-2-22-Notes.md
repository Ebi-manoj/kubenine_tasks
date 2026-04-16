
## CloudWatch Logs Structure

CloudWatch Logs organizes log data in a hierarchical structure:

### 1. Log Group

- A **logical container** for related logs
- Represents a **category of logs** (e.g., system logs, access logs, error logs)
- Controls:
    - Retention policy (e.g., 7 days)
    - Access permissions (IAM)

📌 Example:

- `task-2-22-system-logs`
- `task-2-22-nginx-access-logs`

---

### 2. Log Stream

- A **single source of logs** within a log group
- Typically represents **one EC2 instance**
- Separates logs from different instances

📌 Example:

- `i-123abc` (EC2 instance 1)
- `i-456def` (EC2 instance 2)

---

### 3. Log Events

- The **actual log entries (lines)**
- Each event contains:
    - Timestamp
    - Log message

📌 Example:
```
GET /index.html 200
```

---

### 🧠 Key Understanding

- **Log Group → What type of logs?**
- **Log Stream → Which resource generated them?**
- **Log Events → Actual log data**
---

 **“Why centralized logging?”**

 In auto-scaled environments, instances are ephemeral and can be terminated at any time, causing local logs to be lost. Centralized logging ensures logs are persisted independently of the instance, enabling reliable debugging, monitoring, and analysis across distributed systems without relying on SSH access.

---

## CloudWatch Agent for Logs

### What the Agent Does

- Runs on EC2 and **streams log files to CloudWatch Logs**
- Continuously **tails log files** (reads new lines only)
- Sends logs to:
    - Log Group → category
    - Log Stream → instance source

---

### How Log Shipping Works

```text
Log file (/var/log/...)
   ↓
CloudWatch Agent (tailing)
   ↓
CloudWatch Logs
```

---

### Configuration (Key Idea)

You define:
- **file_path** → which log file to monitor
- **log_group_name** → where to send logs
- **log_stream_name** → usually `{instance_id}`

```json
{
  "file_path": "/var/log/nginx/access.log",
  "log_group_name": "task-2-22-nginx-access-logs",
  "log_stream_name": "{instance_id}"
}
```

---
