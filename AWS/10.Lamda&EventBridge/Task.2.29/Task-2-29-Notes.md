
# AWS Lambda & EventBridge — Study Notes

## 1. AWS Lambda

### What It Is
- **Serverless compute** — run code without managing servers
- AWS handles provisioning, scaling, shutdown
- Pay only per request + per millisecond of execution
- Idle = $0

### Anatomy of a Lambda Function
- **Handler function** — entry point AWS calls (e.g., `lambda_handler`)
- **`event` parameter** — JSON describing what triggered it (S3 info, HTTP request, etc.)
- **`context` parameter** — runtime info (request ID, remaining time, memory, log stream)

```python
def lambda_handler(event, context):
    return {"statusCode": 200, "body": "OK"}
```

### Triggers (Event Sources)
- **S3** — file uploaded/deleted
- **EventBridge** — schedule or AWS event
- **API Gateway** — HTTP requests
- **SNS / SQS** — notifications / queue messages
- **DynamoDB Streams** — row changes
- **Kinesis** — streaming data
- **CloudWatch Logs** — log patterns

### IAM Execution Role
- Lambda **assumes this role** when it runs
- Grants permissions (write logs, read S3, etc.)
- **Without it, Lambda cannot run** — not even write logs
- Minimum: `AWSLambdaBasicExecutionRole` (CloudWatch Logs access)

### CloudWatch Logs
- All `print()` / `console.log()` output → CloudWatch automatically
- Log Group: `/aws/lambda/<function-name>`
- First place to debug failures

### Pricing
- **Per request:** ~$0.20 per 1M requests (1M/month free)
- **Per compute:** GB-seconds (memory × duration)
- Higher memory = more CPU → can reduce total cost
- Free tier: 1M requests + 400,000 GB-seconds/month

### Cold Start vs Warm Start
- **Cold start:** AWS spins up new environment, loads runtime, imports code (~100ms–1s)
- **Warm start:** reuses existing environment (~0 overhead)
- Environments stay warm ~5–15 min after last invocation
- Matters for: low-traffic, latency-sensitive APIs
- Mitigation: Provisioned Concurrency

### Hard Limits
| Limit | Value |
|---|---|
| Max execution time | **15 minutes** |
| Memory | 128 MB – 10 GB |
| Deployment zip (direct) | 50 MB |
| Deployment unzipped | 250 MB |
| Container image | 10 GB |
| `/tmp` storage | 512 MB – 10 GB |
| Concurrent executions | 1,000/region (soft) |
| Sync payload | 6 MB |
| Async payload | 256 KB |

---

## 2. Amazon EventBridge

### What It Is
- Serverless **event bus** — routes events across AWS
- Acts as AWS's central "post office" for events
- Listens to AWS services, schedules, and custom apps

### EventBridge vs SNS vs SQS
| Service | Purpose |
|---|---|
| **SNS** | Pub/sub fan-out to many subscribers |
| **SQS** | Queue (buffer) for decoupling producer/consumer |
| **EventBridge** | Content-based routing + AWS service integration |

### Event Bus
- **Default bus** — auto-created; receives AWS service events
- **Custom bus** — for your own applications
- **Partner bus** — SaaS events (Datadog, Shopify, etc.)

### Rules
- A rule = **pattern/schedule + targets** (up to 5 targets each)
- Two types:

**(a) Scheduled Rules**
```
rate(5 minutes)
rate(1 hour)
cron(0 9 * * ? *)    # daily 9 AM UTC
```

**(b) Event Pattern Rules** (JSON filter)
```json
{
  "source": ["aws.ec2"],
  "detail-type": ["EC2 Instance State-change Notification"],
  "detail": { "state": ["stopped"] }
}
```
- Arrays = OR
- Missing fields = don't care
- Advanced: `anything-but`, `numeric`, `prefix`, `exists`

### AWS Services That Auto-Publish Events
- EC2 (state changes)
- S3 (object created/deleted)
- CodePipeline (stage changes)
- Auto Scaling
- CloudWatch Alarms
- GuardDuty (security findings)
- AWS Health

### Targets
- Lambda, SNS, SQS, Step Functions
- Kinesis, ECS tasks, EC2 actions
- SSM Run Command, API destinations (external HTTPS)
- Another EventBridge bus (cross-account/region)
- Needs permission: resource policy OR IAM role

---

## 3. The Power Combo

```
Event Source → EventBridge Rule → Lambda
```

**Common patterns:**
- `rate(1 day)` → Lambda cleans old S3 files
- EC2 `RunInstances` → Lambda auto-tags instance
- GuardDuty finding → Lambda posts to Slack
- CodePipeline failure → Lambda notifies team

---

