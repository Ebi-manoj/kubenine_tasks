
### 1. What is the purpose of IAM groups?

IAM groups are used to manage permissions for multiple users in a simple and organized way. Instead of assigning permissions individually, you attach policies to the group, and all users in that group inherit those permissions automatically

### 2. Why should policies not be attached directly to users in production?

Attaching policies directly to users becomes hard to manage as the number of users grows. It increases the risk of mistakes, like forgetting to remove access or giving inconsistent permissions across users

### 3. What happens when a user is removed from a group?

They immediately lose all the permissions that came from that group

### 4. Can a user belong to multiple groups? How are permissions evaluated in that case?

Yes, a user can belong to multiple groups at the same time. AWS combines all allowed permissions from those groups, but if any policy has an explicit deny, it overrides all allows

### 5. If one group allows `s3:PutObject` and another group explicitly denies it, what happens?

The action will be denied. In AWS, an explicit deny always takes priority over any allow

### 6. How does group-based IAM reduce mistakes during employee onboarding and offboarding?

With groups, you just add a new user to the right group to give them the correct permissions, which avoids manual errors. When someone leaves, removing them from the group instantly removes all their access, making the process quick and secure