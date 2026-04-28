
### 1. What is Infrastructure as Code and why is it better than manual provisioning?

IaC means managing our infrastructure using code instead of setting things up manually in the console. It’s better because it’s repeatable, can use git for managing versions, and reduces human errors. We can recreate the same setup anytime with consistency.
### 2. What does `terraform plan` do and why should you always run it before `apply`?

It shows what changes Terraform is going to make before actually doing them. It helps you catch mistakes early and understand the impact of your changes
### 3. What is the Terraform state file? What information does it store?

The state file keeps track of what resources Terraform has created and their current state. It stores mappings between your code and real infrastructure. Without it, Terraform won’t know what already exists.
### 4. What happens if you delete the state file but the resources still exist in AWS?

Terraform will think nothing exists and may try to recreate everything from scratch. This can lead to duplicate resources or conflicts. Basically, Terraform loses track of our infrastructure.
### 5. Why do we store state in S3 instead of keeping it local?

Storing state in S3 allows multiple team members to access the same state safely. It also provides durability and prevents data loss if our local machine fails.
### 6. What is state locking and what problem does DynamoDB solve?

State locking prevents multiple people from making changes at the same time. DynamoDB is used to lock the state file so only one operation runs at a time. This avoids conflicts and corruption of the state.
### 7. What is the difference between `terraform.tfvars` and `variables.tf`?
variables.tf is where we define variables and their structure. terraform.tfvars is where we assign actual values to those variables.
### 8. If you run `terraform apply` and it fails halfway through, what happens to the resources that were already created?

The resources that were successfully created will remain in our infrastructure. Terraform will record them in the state file. On the next run, it will continue from where it failed instead of starting over.