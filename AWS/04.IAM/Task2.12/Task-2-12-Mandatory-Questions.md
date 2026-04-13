
**1. Why should the root user never be used for daily operations?**  
The root user has unrestricted access to everything in the AWS account, which makes it extremely risky to use regularly. If its credentials are compromised, the entire account can be taken over. It’s best to use it only for critical tasks and rely on IAM users for daily work.


**2. What is the difference between an IAM user and an IAM role?**  
An IAM user is a permanent identity with long-term credentials like a username and password. An IAM role, on the other hand, provides temporary credentials and is assumed when needed. Roles are commonly used for services or secure access without storing credentials.


**3. What is the difference between an AWS managed policy and a customer-managed policy?**  
AWS managed policies are created and maintained by AWS, offering predefined permissions for common use cases. Customer-managed policies are created by you, giving more control and customization. The latter is preferred when you need precise, least-privilege access.


**4. What happens when one policy allows an action and another policy explicitly denies it?**  
In AWS, an explicit deny always overrides any allow. Even if a policy grants permission, the action will still be blocked if another policy denies it. This rule ensures stronger control over sensitive operations.


**5. Why is Administrator Access dangerous in production?**  
Administrator Access gives full access to all AWS services and resources, which increases the risk of accidental or malicious changes. A small mistake or compromised account can lead to serious damage like data loss or high costs. That’s why it should be avoided in production environments.



**6. What is least privilege and how do you apply it in practice?**  
Least privilege means giving only the minimum permissions required to perform a task. In practice, you apply it by granting specific actions instead of broad access and removing unused permissions. This reduces the risk of misuse or security breaches.



**7. Why is MFA critical for the root account? What risk does it mitigate?**  
MFA adds an extra layer of security by requiring a second verification step beyond the password. For the root account, it prevents unauthorized access even if the password is stolen. This protects against full account takeover.


**8. If an IAM user's password is compromised but MFA is enabled, can the attacker access the console?**  
No, the attacker cannot access the console with just the password. They would also need the MFA code, which acts as a second authentication factor. This significantly reduces the risk of unauthorized access.