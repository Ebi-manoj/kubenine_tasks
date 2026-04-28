
### 1. What is the difference between an IAM role trust policy and a permissions policy?

A trust policy defines who is allowed to assume a role, like allowing Lambda service to use it. A permissions policy defines what actions are allowed after the role is assumed, like accessing S3 or writing logs
### 2. Why does Lambda use a role instead of an IAM user with access keys?

Lambda uses a role because it provides temporary credentials that are automatically managed by AWS. Using an IAM user with access keys would require storing secrets in code, which is insecure.
### 3. What is a resource-based policy, and why does S3 need permission to invoke Lambda?

A resource-based policy is attached directly to a resource and defines who can access it. S3 needs permission to invoke Lambda because it is the service calling the function, so Lambda must explicitly allow it
### 4. What does the S3 event payload look like? How do you extract the object key from it?

The S3 event payload is a JSON structure with a Records array containing details about the event. The object key can be extracted using event 'Records'0's3'object''key'.
### 5. If you upload 3 files at the same time, how many times does Lambda trigger?

If 3 files are uploaded at the same time, Lambda triggers 3 separate times, once for each file. Depending on scaling, they may run in parallel.
### 6. Where do Lambda logs go and how do you find them in CloudWatch?

Lambda logs go to Amazon CloudWatch Logs automatically. They can be found under the log group with Lambda function name.
### 7. Why is `AmazonS3ReadOnlyAccess` dangerous on a Lambda role even if you only need read access?

AmazonS3ReadOnlyAccess is dangerous because it gives read access to all S3 buckets in the account. Even if only one bucket is needed, this violates least privilege and increases security risk.