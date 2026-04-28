
### 1. What is the difference between AWS managed keys and customer managed keys?

AWS managed keys are created and controlled entirely by AWS, so we don’t get much control over permissions or lifecycle. Customer managed keys are created by us, giving full control over access policies, rotation, and usage.
### 2. Why might an organization prefer customer managed keys over AWS managed keys?

Customer managed keys allow fine-grained control over who can encrypt/decrypt data and provide better auditability. This is important for security, compliance, and production environments
### 3. What is a KMS key policy and how does it differ from an IAM policy?

A KMS key policy is attached directly to the key and defines who can use or manage that specific key. An IAM policy is attached to users or roles and defines what actions they are allowed to perform.
### 4. What happens if a user tries to read an S3 object encrypted with a KMS key they don't have `kms:Decrypt` permission for?

Even if the user has permission to read the S3 object, they won’t be able to access its contents. KMS will deny the decryption request, so the data remains unreadable.
### 5. Why is using a single KMS key for multiple services (S3, SSM) a valid approach?

Using one key for multiple services centralizes control and simplifies management. It ensures consistent security policies across S3, SSM, and other services
### 6. What is encryption at rest and why does it matter for data stored in S3 and Parameter Store?

Encryption at rest means data is stored in an encrypted form when saved on disk. This protects data from unauthorized access if storage is compromised.
### 7. If you delete a KMS key, what happens to the data encrypted with it?

Once a KMS key is deleted, any data encrypted with it becomes permanently unrecoverable. AWS enforces a waiting period before deletion to prevent accidental loss