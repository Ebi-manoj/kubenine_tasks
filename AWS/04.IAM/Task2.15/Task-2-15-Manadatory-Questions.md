
### 1. What is AWS STS and what does it do?

AWS Security Token Service is a service that provides temporary security credentials to access AWS resources. It allows users or services to assume roles instead of using permanent access keys.
### 2. What does the AssumeRole action return?

returns temporary credentials: Access Key ID, Secret Access Key, Session Token, and an expiration time.
### 3. Why are temporary credentials safer than long-lived access keys?

Temporary credentials automatically expire after a short time, reducing the risk if they are leaked.  Long lived keys remain valid until manually revoked, making them more dangerous
### 4. What is the difference between the trust policy and the permission policy on a role?

The trust policy defines _who can assume the role_, while the permission policy defines _what actions the role can perform_. Both are required for secure role assumption
### 5. What happens when temporary credentials expire?

Once temporary credentials expire, all API requests using them fail with an Expired Token error
### 6. If the trust policy allows a user but the user doesn't have `sts:AssumeRole` permission, can they assume the role? Why?

No, the user cannot assume the role. Both the trust policy and the user’s permission policy must allow the action, otherwise AWS denies the request.
### 7. Why is the session token mandatory when using temporary credentials?

The session token proves that the credentials are temporary and issued by STS, adding an extra layer of security. Without it, AWS will reject the request
### 8. How does STS relate to what happens behind the scenes when EC2 assumes a role (Task 2.13)?

When Amazon EC2 assumes a role, it actually calls STS in the background to get temporary credentials. These credentials are then stored in the instance metadata and used automatically by applications.