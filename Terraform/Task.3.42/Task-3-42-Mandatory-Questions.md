
### 1. What is the difference between a customer managed policy, an inline policy, and an AWS managed policy?

A customer managed policy is created and controlled by us in our AWS account, so we can fully customize and reuse it. An inline policy is directly attached to a single role or user and cannot be reused elsewhere. An AWS managed policy is created by AWS and is maintained by them for common use cases.
### 2. When would you use an inline policy vs a managed policy?

We use an inline policy when the permission is tightly coupled to one specific role and shouldn’t be reused. We use a managed policy when the same permissions need to be shared across multiple roles or users.
### 3. What is a trust policy (assume role policy) and why does a role need one?

A trust policy defines who is allowed to assume a role. It is required because without it, no service or user can use that role
### 4. What Terraform resource type do you use for an inline policy vs an attached policy?
we use aws_iam_role_policy for inline policies. For attached (managed) policies, we use aws_iam_role_policy_attachment.
### 5. What is a `data` source in Terraform? How is it different from a `resource`?

A data source in Terraform is used to read existing resources without creating them. A resource block is used to create and manage infrastructure.
### 6. If you delete the managed policy resource from your `.tf` file and run `apply`, what happens to the policy in AWS?

Terraform will destroy that policy from AWS if it is managed by Terraform. This happens because Terraform tries to match the actual state with the configuration.
### 7. You have the same permissions defined in the inline policy AND the managed policy. What happens — do they conflict, or do they combine?

The permissions do not conflict, they combine. AWS follows a union model where all allowed permissions are merged together. So we effectively get the sum of permissions from both policies unless there is an explicit deny.