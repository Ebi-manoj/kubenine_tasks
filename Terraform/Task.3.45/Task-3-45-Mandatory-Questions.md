
### 1. Why do we store the Slack webhook URL in SSM Parameter Store instead of putting it in `terraform.tfvars`?

We store the Slack webhook URL in SSM Parameter Store because it’s a secret, and keeping secrets in terraform.tfvars risks exposing them in code, state files, or version control. Using SSM lets us centralize and secure sensitive values while still allowing Terraform to read them safely.
### 2. What is the difference between a `String` and `SecureString` parameter in SSM?

A String parameter stores plain text, while a SecureString encrypts the value using AWS KMS. We use SecureString for secrets like webhook URLs so the value is protected both at rest and when accessed.
### 3. Explain the full notification pipeline: what happens from the moment CPU exceeds the threshold to the message appearing in Slack?

When CPU exceeds the threshold, CloudWatch detects the metric breach and moves the alarm into ALARM state. It then sends a notification to an SNS topic, which triggers a Lambda function that formats the message and sends it to Slack via the webhook.
### 4. What is an SNS topic? Why is it between CloudWatch and Lambda instead of CloudWatch calling Lambda directly?

An SNS topic is a messaging hub that allows multiple subscribers to receive the same event. We place it between CloudWatch and Lambda to decouple the system, making it more flexible and scalable instead of tightly binding CloudWatch directly to one Lambda.
### 5. You set the alarm threshold to 2%. In production, what would be a realistic threshold and why?

A realistic production threshold would be around 70–80 percent sustained CPU usage. This avoids noise from normal fluctuations and ensures alerts only fire when the system is genuinely under stress.
### 6. What are the three states of a CloudWatch alarm? What triggers the transition between them?

The three states are OK, ALARM, and INSUFFICIENT_DATA. Transitions happen based on whether the metric crosses the defined threshold or if there is not enough data to evaluate the condition.
### 7. If you delete the SSM parameter but the Terraform state still references it, what happens on the next `terraform plan`?

If we delete the SSM parameter but Terraform still references it, the next plan will fail because the data source cannot find the parameter.