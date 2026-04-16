
## 📌 Target Groups & Listeners (AWS ALB)

### 🔹 Target Group

- Logical group of backend targets (EC2 / IP / Lambda)
- ALB forwards requests to targets in a target group
- Config:
    - Protocol: HTTP/HTTPS
    - Port: e.g., 80
    - Health check path: `/`
- Maintains:
    - Target IP + port
    - Health status (Healthy / Unhealthy)

#### 🔬 How it works

- ALB selects **only healthy targets**
- Uses routing algorithm (round robin by default)
- Sends request → target → returns response via ALB

---

### 🔹 Listener

- Process that **listens on a port + protocol** (e.g., HTTP:80)
- Defines **rules to route traffic**

#### 🔬 Flow

1. Receive request
2. Evaluate rules (path, host, headers)
3. Forward to target group (or redirect/respond)

#### Example

- `/api` → TG-API
- `/images` → TG-IMAGES
- Default → TG-DEFAULT

---

### 🔹 Health Checks

- ALB sends periodic HTTP requests (default ~30s)
- Example:
    
    GET /
    
- Checks:
    - Response code (200 OK)
    - Response time

#### Decision Logic

- Success ≥ threshold → **Healthy**
- Failure ≥ threshold → **Unhealthy**