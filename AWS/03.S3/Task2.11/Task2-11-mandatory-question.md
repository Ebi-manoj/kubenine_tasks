### 1. What is the difference between an IAM policy and a bucket policy?

An IAM policy is **identity-based** and defines what actions a user, role, or group can perform across AWS resources. A bucket policy is **resource-based** and controls who can access a specific S3 bucket and what actions they can perform on it. Both must allow access for a request to succeed.

---

### 2. If a bucket policy allows public read but the Public Access Block is enabled, what happens?

The request will be **denied** because Public Access Block overrides any public permissions defined in the bucket policy. Even if the policy allows `"Principal": "*"`, AWS ignores it due to the safety restriction. This prevents accidental public exposure.

---

### 3. What does "explicit deny overrides allow" mean in practice?

If any policy (IAM, bucket policy, or others) explicitly sets `"Effect": "Deny"`, the request is denied even if other policies allow it. AWS always prioritizes **deny over allow** during evaluation. This ensures strict enforcement of security boundaries.

---

### 4. Why must versioning be enabled on both buckets before configuring replication?

Replication relies on tracking **object versions** to identify new and updated objects. Without versioning, S3 cannot determine what changes need to be replicated. Therefore, both source and destination buckets must have versioning enabled.

---

### 5. What IAM component is required for S3 replication to work?

An **IAM role** is required for replication, which S3 assumes to perform actions on your behalf. This role grants permissions to read from the source bucket and write to the destination bucket. Without it, replication cannot occur.

---

### 6. Are existing objects replicated when you enable CRR? Why or why not?

No, existing objects are **not replicated automatically** because CRR is event-driven. It only applies to new objects or updates after replication is enabled. Older objects are ignored unless you use batch replication.

---

### 7. What is the biggest security risk with S3 buckets in production, and how do the permission layers protect against it?

The biggest risk is **accidental public exposure of sensitive data** due to misconfigured permissions. Public Access Block prevents public access, bucket policies control resource-level access, and IAM policies restrict user actions. Together, they enforce layered security and prevent data leaks.