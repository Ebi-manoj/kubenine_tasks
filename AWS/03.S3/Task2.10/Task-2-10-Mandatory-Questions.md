
### 1. What does versioning protect against that "Block Public Access" does not?

Both are different concepts , versioning enable you to prevent unintentional activities done to a object by giving a roll back options.
Block public access will prevent access of your bucket to the public directly. 
### 2. What is a delete marker and why doesn't a delete actually remove the object?

Delete marker is an another version of you object , its just added a version when you delete the object showing that it is deleted and cannot be accessed. But as i said its just a another version and you can recover the original object my removing the delete marker.
### 3. Can you disable versioning once it is enabled? What happens instead?

No, you cannot completely disable versioning once it’s enabled. You can only suspend it, which stops new versions from being created. However, all existing versions remain stored and accessible.
### 4. What problem do lifecycle rules solve in production?

Lifecycle rules automate data management and cost optimization in S3. They move older data to cheaper storage or delete unused versions automatically. This prevents storage costs from growing data over the time.
### 5. What is encryption at rest and why is it important even inside AWS?

Encryption at rest means your data is stored in an encrypted format on disk. Even within AWS, this protects your data if there’s unauthorized access to the underlying storage. It ensures that the data remains unreadable without proper decryption keys
### 6. What is the difference between SSE-S3 and SSE-KMS? When would you choose KMS?

SSE-S3 uses AWS-managed keys and is simple to use with minimal setup. SSE-KMS lets you control the keys, provides audit logs, and allows fine-grained access control
### 7. If versioning is enabled and you have 100 versions of a 1GB file, how much storage are you paying for?

we are charged for all versions, so 100 versions of a 1GB file means 100GB of storage. S3 stores each version as a full object, not a difference.