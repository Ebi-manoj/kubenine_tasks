
### 1. Why should secrets never be stored in application code or configuration files?

Secrets in code or config files can easily leak through Git history, logs, or shared images. Once exposed, they’re hard to rotate and can compromise the entire system. Keeping them external reduces blast radius and improves control.
### 2. How does Secrets Manager encrypt secrets at rest?

It uses AWS KMS to encrypt secrets before storing them. The data is stored as encrypted, and only decrypted when an authorized request is made. This ensures the plaintext is never persistently stored
### 3. What IAM action is required to retrieve a secret value?

The required IAM action is secretsmanager:GetSecretValue. Without this permission, the application or EC2 instance cannot read the secret.
### 4. Why should IAM policies avoid `"Resource": "*"` for secrets access?

Using "Resource": "" gives access to all secrets in the account, which violates the principle of least privilege. If one component is compromised, it could expose every secret
### 5. What is the difference between Secrets Manager and SSM Parameter Store?

Secrets Manager is designed specifically for sensitive data, with built-in rotation and more advanced security controls. Parameter Store can store secrets too, but it’s more general purpose and lacks advanced rotation features by default.
### 6. How would a containerized application retrieve secrets at runtime without hardcoding them?

The container uses an IAM role via instance role to call Secrets Manager at runtime using an SDK or CLI. It fetches the secret dynamically when needed instead of storing it in the image.
### 7. What happens if an EC2 instance's IAM role does not have permission to read a specific secret?

The request to Secrets Manager will fail with an Access Denied Exception. The secret will not be returned, even if it exists.