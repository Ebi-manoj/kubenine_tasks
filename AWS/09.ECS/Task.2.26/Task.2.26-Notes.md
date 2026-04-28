
## 🔐 KMS Integration: S3 vs SSM (Short Notes)

### Core Idea

**AWS KMS does NOT store your data or “remember” anything.**  
It only performs cryptographic operations when requested.

---

## 🪣 S3 + KMS (Envelope Encryption)

- Used for **large data (files)**
- KMS does **NOT encrypt the file directly**

### Flow:

1. S3 requests a **data key** from KMS
2. KMS returns:
    - Plaintext data key
    - Encrypted data key
3. S3:
    - Encrypts file using plaintext key
    - Stores:
        - Encrypted file
        - Encrypted data key
4. Plaintext key is discarded

### Decryption:

- S3 sends encrypted data key → KMS
- Requires `kms:Decrypt`
- KMS returns plaintext key → S3 decrypts file

✅ Reason: Performance + scalability  
❗ KMS only protects the **data key**, not the file

---

## 🔐 SSM Parameter Store + KMS (Direct Encryption)

- Used for **small values (secrets/configs)**

### Flow:

1. Value → sent to KMS
2. KMS encrypts it directly
3. Encrypted blob is stored

### Decryption:

- Encrypted value → KMS → plaintext

✅ No data key needed  
✅ Simpler flow

---
## 🧠 Final Insight

- **SSM:** KMS encrypts the data itself
- **S3:** KMS encrypts a key → key encrypts data
👉 That’s why S3 must store an **encrypted data key**, but SSM doesn’t.

