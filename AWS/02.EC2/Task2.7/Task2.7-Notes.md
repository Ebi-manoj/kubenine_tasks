
# 🧠 EC2 Boot, Bootstrapping, User Data & cloud-init 

---

## 1. 🔹 What is Boot?

**Boot (Booting)** = Process of starting a system from powered-off → usable OS.

### Boot Sequence:

1. Firmware (BIOS/UEFI) starts
    
2. Bootloader loads (e.g., GRUB)
    
3. Kernel loads into memory
    
4. Kernel initializes:
    
    - CPU
        
    - Memory
        
    - Devices
        
5. Kernel starts **init system (systemd)**
    
6. System services start
    

✅ Result: OS becomes usable

---

## 2. 🔹 What is Bootstrapping?

**Bootstrapping** = Configuring the system _after boot_ to make it application-ready.

### Difference:

|Concept|Meaning|
|---|---|
|Boot|Start OS|
|Bootstrapping|Prepare OS for use|

### Example:

- Boot → Linux starts
    
- Bootstrapping → Install Nginx, configure server
    

👉 Boot gives a machine  
👉 Bootstrapping gives a **ready server**

---

## 3. 🔹 What is User Data?

**User Data** = Script provided during EC2 launch that runs automatically on first boot.

### Used for:

- Installing software
    
- Configuring environment
    
- Starting services
    

### Example:

```bash
#!/bin/bash
yum update -y
yum install nginx -y
systemctl start nginx
systemctl enable nginx
```

---

## 4. 🔹 When does User Data run?

Runs during:  
👉 **Instance boot (pending → running)**

### Important Rules:

|Action|User Data Runs?|
|---|---|
|First launch|✅ Yes|
|Stop → Start|❌ No|
|Reboot|❌ No|

---

## 5. 🔹 What is cloud-init?

**cloud-init** = Initialization service inside the EC2 instance that executes User Data and handles setup.

### Key Points:

- Installed in AMIs (Amazon Linux, Ubuntu)
    
- Triggered during boot via systemd
    
- Fetches metadata and User Data
    
- Executes scripts
    

---

## 6. 🔹 cloud-init Behavior

### Execution Stages:

1. Init stage
    
2. Config stage
    
3. Final stage (User Data runs here)
    

### Properties:

|Property|Value|
|---|---|
|Background service|✅ Yes (during boot)|
|Long-running daemon|❌ No|
|Runs forever|❌ No|
|Runs every boot|❌ No (default: first boot only)|

---

## 7. 🔹 Is cloud-init the main process?

❌ No

### Main process:

👉 **systemd (PID 1)**

### Process hierarchy:

```
systemd (PID 1)
   ├── cloud-init
   ├── sshd
   ├── network services
   └── other services
```

---

## 8. 🔹 Internal EC2 Boot Flow (End-to-End)

### Step-by-step:

1. EC2 launched → `pending`
    
2. AWS:
    
    - Creates VM (hypervisor)
        
    - Attaches storage & network
        
    - Injects metadata (including User Data)
        
3. OS boots:
    
    - Kernel loads
        
    - systemd starts
        
4. systemd starts services:
    
    - network
        
    - ssh
        
    - cloud-init
        
5. cloud-init:
    
    - Fetches data from:
        
        ```
        http://169.254.169.254
        ```
        
    - Executes User Data
        
6. Script runs:
    
    - Installs software
        
    - Starts services
        
7. cloud-init exits
    
8. Instance state → `running`
    

---

## 9. 🔹 What happens if User Data fails?

❗ Instance still launches successfully

### Outcome:

- EC2 state → ✅ Running
    
- Script → ❌ Failed (partially/fully)
    

### Example failure:

- Nginx install fails → website not accessible
    

---

## 10. 🔹 How to debug User Data?

Check logs:

```bash
sudo cat /var/log/cloud-init-output.log
```

Contains:

- Script output
    
- Errors
    
- Execution logs
    

---

## 11. 🔹 Can User Data run again?

By default:  
❌ No (runs only once)

### Manual rerun:

```bash
sudo cloud-init clean
sudo cloud-init init
```

---

## 12. 🔹 Why AWS designed it this way?

Supports:

- Automation
    
- Immutable infrastructure
    
- Infrastructure as Code
    

### Best Practice:

❌ Don’t fix servers manually  
✅ Recreate with correct User Data

---

