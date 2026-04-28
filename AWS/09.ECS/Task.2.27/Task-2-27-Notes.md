
## Request Flow

```
STEP 1 — DNS resolution
──────────────────────────────────────────────────────────
Browser: DNS lookup for task-2-27-alb-abc123.us-east-1.elb.amazonaws.com
Route 53 returns: [54.211.10.5, 54.211.10.6]  (ALB's public IPs, one per AZ)

STEP 2 — TCP + HTTP request
──────────────────────────────────────────────────────────
Browser → 54.211.10.5:80
  GET / HTTP/1.1
  Host: task-2-27-alb-abc123.us-east-1.elb.amazonaws.com

STEP 3 — Internet gateway → ALB
──────────────────────────────────────────────────────────
Request enters AWS via the Internet Gateway of the default VPC.
Lands on the ALB node in the public subnet in us-east-1a.
ALB SG check: inbound 80 from 0.0.0.0/0 → ALLOWED ✓

STEP 4 — ALB listener
──────────────────────────────────────────────────────────
Listener on port 80 sees the request.
Rule: "forward to target group task-2-27-tg"

STEP 5 — Target selection
──────────────────────────────────────────────────────────
Target group has 2 healthy targets:
  172.31.48.15:8501 (Task A)
  172.31.52.78:8501 (Task B)

ALB picks one using "round robin" (default algorithm).
Picks Task A at 172.31.48.15:8501.

STEP 6 — ALB → Fargate task
──────────────────────────────────────────────────────────
ALB opens a new TCP connection from its private IP (in a public subnet)
to 172.31.48.15:8501.

Route table check: ALB is in VPC, Task A is in same VPC → direct routing.
ECS SG check: inbound 8501 from task-2-27-alb-sg → ALLOWED ✓
(Because the ALB's SG matches, not because of IP range.)

STEP 7 — Container receives request
──────────────────────────────────────────────────────────
ENI on Task A receives the packet on port 8501.
Docker routes it to the Streamlit process listening on 0.0.0.0:8501.
Streamlit handles the request, returns HTTP 200 with HTML.

STEP 8 — Response path (reverse)
──────────────────────────────────────────────────────────
Streamlit → Task A ENI → ALB node → user's browser.
```


