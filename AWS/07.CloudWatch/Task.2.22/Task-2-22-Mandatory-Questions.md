
### 1. What is the difference between a Log Group and a Log Stream?
A log group is a container for a specific type of logs like Nginx access logs, while a log stream represents logs coming from a single source, usually one EC2 instance.
### 2. Why is centralized logging important in production?

In production, instances are dynamic and can be terminated anytime, so local logs can be lost. Centralized logging ensures logs are stored in one place, making debugging, monitoring, and auditing reliable.
### 3. Why should production systems avoid relying on SSH for log debugging?

SSH doesn’t scale when you have multiple instances, and the problematic instance might already be terminated.
### 4. What is log retention and why does it matter for both cost and compliance?

Log retention defines how long logs are stored before being deleted. It helps control storage costs and ensures logs are kept only as long as required for auditing .
### 5. What types of issues are easier to detect in logs vs metrics?

Logs help identify exact root causes like errors, failed requests, or exceptions. Metrics are better for spotting trends or spikes like high CPU or increased error rates.
### 6. How does the CloudWatch Agent collect and ship logs from an EC2 instance?

The agent continuously reads specified log files like access.log and sends new log entries to CloudWatch Logs. It acts like a background process that streams logs in near real-time
### 7. What is the difference between metrics and logs in terms of what they tell you about a problem?

Metrics tell you that something is wrong (like high CPU or error spikes), while logs explain why it happened by showing detailed events and messages.