
### 1. What is `user_data` in EC2? At what point in the instance lifecycle does it run?

user data is a script we provide to EC2 that runs automatically when the instance boots for the first time. We typically use it to install software or configure the instance. It runs during the initial launch phase, handled by cloud-init.
### 2. If your user_data script fails, how would you debug it? Where do you find the logs?

We check the logs inside the instance, mainly /var/log/cloud-init-output.log and /var/log/cloud-init.log.
### 3. What is `terraform_remote_state` and why is it better than hardcoding a VPC ID?

terraform_remote_state lets us fetch outputs (like VPC ID) from another Terraform stack. This avoids hardcoding values and keeps things dynamic and reusable
### 4. Why did we disable the NAT Gateway? What would happen if an instance in a private subnet tried to reach the internet now?

We disabled NAT Gateway mainly to reduce cost. Without it, instances in private subnets cannot access the internet. So things like package updates or external API calls will fail.
### 5. You used a `data "aws_ami"` to find the AMI. Why is this better than hardcoding an AMI ID?

Using data source helps us always fetch the latest valid AMI automatically. Hardcoding AMI IDs can break over time or vary by region. This makes our setup more maintainable and flexible.
### 6. Why should SSH (port 22) be restricted to your IP and not open to `0.0.0.0/0`?

Opening SSH to the world is a major security risk and invites brute-force attacks. Restricting it to our IP ensures only we can access the instance. It follows the principle of least privilege
### 7. If you terminate the EC2 instance from the AWS Console (not Terraform), what happens when you run `terraform plan` next?

Terraform will detect that the resource is missing from the real infrastructure. On the next plan, it will show that it needs to recreate the instance. This is because Terraform state and actual infra are now out of sync.
### 8. What is the difference between the NACL rules (from Task 3.43) and the Security Group rules you created here? Why do we need both?

Security Groups are stateful and act at the instance level, while NACLs are stateless and work at the subnet level.We use both for layered security.