
**1. Why does AWS provide CPU metrics by default but not memory or disk?**  
AWS can expose CPU, network, and disk I/O metrics because they are visible at the hypervisor layer, but memory and filesystem usage exist inside the guest OS, which AWS cannot access due to instance isolation and security boundaries.

---

**2. What is the difference between hypervisor-level and OS-level metrics?**  
Hypervisor-level metrics are collected externally by AWS (like CPU utilization and network traffic), while OS-level metrics require in-instance access (like memory usage and disk space), which must be collected using an agent.

---

**3. What is the CloudWatch Agent and why is it needed for memory and disk monitoring?**  
The CloudWatch Agent is an in-instance service that collects OS-level telemetry such as memory, disk, and logs, and publishes it to CloudWatch since AWS cannot natively access this data from outside the instance.

---

**4. What is the difference between CloudWatch metrics and logs?**  
Metrics are structured numerical time-series data used for monitoring and alerting, whereas logs are unstructured or semi-structured textual records that provide detailed context for troubleshooting and root cause analysis.

---

**5. What is a CloudWatch namespace and how are metrics organized?**  
A namespace is a logical container that groups related metrics (e.g., AWS/EC2 or CWAgent), and metrics are uniquely identified using a combination of namespace, metric name, and dimensions to avoid conflicts and enable fine-grained filtering.

---

**6. How can CloudWatch alarms trigger automated actions beyond email?**  
CloudWatch alarms can integrate with other AWS services to trigger actions such as scaling Auto Scaling Groups, invoking Lambda functions, or performing EC2 actions like reboot or termination based on threshold breaches.

---

**7. Why is monitoring critical for production systems?**  
Monitoring provides real-time visibility into system health, enabling proactive alerting, automated scaling, and faster incident response before issues impact end users.

---

**8. What happens if you run a memory-intensive application on an instance without memory monitoring?**  
The application may exhaust available RAM leading to performance degradation or crashes, while remaining undetected since CPU metrics may appear normal, resulting in delayed detection and difficult debugging.