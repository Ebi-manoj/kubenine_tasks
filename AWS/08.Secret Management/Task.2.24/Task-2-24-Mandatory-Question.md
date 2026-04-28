
### 1. What is the difference between Secrets Manager and Parameter Store? When would you use each?
Parameter Store is mainly used to store application configuration like URLs or environment values, while AWS Secrets Manager is built specifically for sensitive data like passwords and API keys.  
In real, we use Parameter Store for general config and Secrets Manager when you need stronger security features like automatic rotation.
### 2. What is a SecureString parameter and when should it be used?

A SecureString is just a parameter that’s stored in encrypted form using AWS Key Management Service.  We use it whenever we are dealing with sensitive values like database passwords or tokens that shouldn’t be stored as plain text.
### 3. Why is hierarchical parameter naming useful for multi-environment deployments?

Using a hierarchical structure helps you keep things organized across different environments like dev, staging, and production. It also makes it easier to fetch multiple parameters at once and control access using IAM policies.
### 4. What IAM permissions are needed to retrieve and decrypt parameters?

To read parameters, you need permissions like ssm:GetParameter or ssm:GetParametersByPath. 
If the parameter is  SecureString, we also need kms:Decrypt to actually see the original value
### 5. Why should application configuration not be hardcoded?

Hardcoding config values makes our app rigid and harder to manage across environments.  
By keeping them external, we can update values without changing code and improve security at the same time
### 6. What is the difference between String, StringList, and SecureString parameter types?

String is for simple plain text values, and StringList is for multiple values separated by commas.  
SecureString is used when the data needs to be encrypted because it’s sensitive.
### 7. How does Parameter Store use AWS KMS for SecureString encryption?

When we store a SecureString, Parameter Store sends the value to AWS Key Management Service to encrypt it before saving. When we retrieve it with decryption, KMS decrypts it and returns the original value securely.