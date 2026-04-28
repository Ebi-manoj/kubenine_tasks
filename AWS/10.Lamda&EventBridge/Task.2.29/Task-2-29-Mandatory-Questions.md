
### Lambda

##### 1. What is the difference between Lambda and EC2? When would you choose one over the other?
Lambda is serverless where we just run code and AWS handles everything, while EC2 gives you full control over a virtual server. We choose Lambda for event driven, short tasks and EC2 for long running or application needs full control .
##### 2. What is a Lambda handler and what arguments does it receive?

A Lambda handler is the main function AWS calls when your function runs. It receives an event object with event data and a context object with runtime details.
##### 3. Why does Lambda need an IAM execution role? What happens if the role has no permissions?

Lambda needs an IAM execution role to access other AWS services securely. If the role has no permissions, the function runs but any AWS service calls will fail.
##### 4. What is a cold start in Lambda and why does it happen?

A cold start happens when Lambda has to create a new execution environment before running our code. This happens because Lambda doesn’t keep environments running all the time
##### 5. Where do Lambda logs go automatically, and why?

Lambda logs are automatically sent to Amazon CloudWatch Logs. This helps with debugging and monitoring without extra setup.
##### 6. What happens if a Lambda function runs longer than its configured timeout?

If a Lambda runs longer than its timeout, AWS stops it immediately. The execution ends and we may lose incomplete work.

### EventBridge

##### 1. What is the difference between a scheduled rule and an event pattern rule?

A scheduled rule runs based on time like a cron job, while an event pattern rule runs when a specific event happens. One is time-driven and the other is event-driven.
##### 2. What is the default event bus and what publishes to it?

The default event bus is built into Amazon EventBridge and receives events from AWS services. Services like EC2 or S3 automatically publish events to it.
##### 3. How does EventBridge know which events to match? What is an event pattern?

EventBridge matches events using an event pattern, which is a JSON filter that checks fields like source or detail. If the event matches, the rule triggers.
##### 4. Can one EventBridge rule have multiple targets? What happens if one target fails?

Yes, one EventBridge rule can have multiple targets. If one target fails, others still run and failures can be retried
##### 5. What is the difference between EventBridge and SNS?

EventBridge is used for routing events based on rules, while SNS is mainly for sending messages to multiple subscribers. EventBridge is smarter filtering, SNS is simple notification pusher.
##### 6. How would you use EventBridge to replace a cron job that runs on an EC2 instance?

To replace a cron job, I create a scheduled rule in EventBridge and connect it to a Lambda. This removes the need for an always-running EC2 instance