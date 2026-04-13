

### 1. What is the difference between object storage (S3) and block storage (EBS)?

Object storage stores data in a form of object with unique id , metadata and actual data bytes . It can be accessed easily via a HTTP protocol and it is designed for high scalability.
But in the case of block storage data stores in a raw blocks and it requires a file system to format and access data efficiently. It is designed for low latency and high performance workloads like databases .

### 2. Why must S3 bucket names be globally unique?

Each files are accessed by a URL consist of s3 bucket name . It is a part of global DNS namespace . So it should be unique so that it will not conflict with other bucket names when users across the world accessing files.

### 3. Why does AWS block public access by default on new buckets?

AWS enables Block Public Access by default to prevent accidental data exposure, which is a common cause of security breaches. It acts as a safeguard to ensure that sensitive data is not unintentionally made accessible to the internet.

### 4. What makes a "folder" in S3 different from a folder on your local filesystem?

S3 uses a flat structure , every files are stored as a object and put into this flat surface , there is no hierarchy there. In S3, a “folder” is just a prefix in the object key, not a real directory structure.

### 5. What storage class would you use for data accessed once a month? What about data you need to keep for 7 years but rarely access?

For data accessed once a month, use S3 Standard-IA, which offers lower storage cost with instant access but includes retrieval fees. For long-term archival , use S3 Glacier

### 6. What is a pre-signed URL and why is it preferred over making an object public?

A pre-signed URL is temporary access to a private files in s3. When it is expired , it cannot access anymore . Its is preferred because it give a controlled access for the private files to users in a temporary way.

### 7. What happens when a pre-signed URL expires?

The file cannot be no longer access , It will show a access denied error message.