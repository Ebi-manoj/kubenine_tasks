### 1. What is the main difference between Security Groups and NACLs?

Security Groups are stateful and acts upon instance level. NACLs are stateless and acts upon subnet level
### 2. What does "stateful" mean in Security Groups?

Stateful means that it can track the connection state , so that any inbound traffic allowed coming can automatically allowed for outbound as well , response will never fail.
### 3. What does "stateless" mean in NACLs?

Stateless means there will be no tracking connection states , so we should explicitly control both inbound and outbound as well
### 4. Which is evaluated first — NACL or Security Group?

NACL will be evaluated first , because it is configured at subnet level .The request will reach subnet first then only moves to instance. So NACL will be evaluated first
### 5. Why are Security Groups preferred in most production setups?

Security Groups are easier to manage and less error prone because they automatically handle return traffic.
### 6. Give one example where a NACL misconfiguration could break an application.

If outbound ephemeral ports 1024–65535 are not allowed, the request reaches the server but the response gets blocked, causing the application to fail