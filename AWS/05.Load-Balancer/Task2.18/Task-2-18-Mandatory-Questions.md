
### 1. What is the main difference between Layer 4 and Layer 7 load balancing?
The main difference is that Layer 4 load balancing works at the TCP/UDP level and only looks at IP and port, while Layer 7 understands HTTP requests like URLs, headers, and hostnames. Because of this, Layer 7 can make smarter routing decisions based on the request content, whereas Layer 4 just forwards connections
### 2. Why can't NLB perform path-based routing?

NLB cannot perform path-based routing because it never reads the HTTP request. It only sees the TCP connection details like port and IP, so it has no idea what path requested.
### 3. When would you use NLB instead of ALB? Give a specific example.

We use NLB when we need very high performance or are working with non-HTTP protocols. For example, in a real-time gaming server or a financial trading system where low latency and handling millions of connections is critical.
### 4. Why is NLB considered higher performance than ALB?

because it does very minimal processing per connection. It simply forwards TCP traffic without inspecting or parsing requests,
### 5. Can NLB terminate SSL/TLS like ALB? What's different?

Yes, NLB can terminate SSL/TLS, but it works differently from ALB. NLB can handle TLS at the connection level, but it still does not inspect HTTP data after decryption, so it cannot do advanced routing like ALB.
### 6. Why is static IP support important in some architectures?

Static IP support is important when external systems or firewalls need a fixed IP to allow traffic. In many enterprise or banking systems, only whitelisted IPs are accepted, so having a stable IP from NLB becomes necessary
### 7. You have a gRPC microservice that needs low latency. Which load balancer would you use and why?

 I would use NLB because gRPC runs over HTTP/2 and benefits from fast, persistent connections. NLB provides lower latency and better performance for such high-throughput communication