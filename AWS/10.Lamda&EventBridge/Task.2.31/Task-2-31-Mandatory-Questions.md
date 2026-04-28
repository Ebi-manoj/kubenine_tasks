
### 1. What is the difference between `rate(1 minute)` and a cron expression in EventBridge?
rate(1 minute) runs the function at a fixed interval continuously, regardless of the actual clock time. A cron expression runs at specific times based on a calendar, like every day at 9 AM or only on weekdays.
### 2. Why is it better to pass the bucket name as an environment variable instead of hardcoding it?

Passing the bucket name as an environment variable makes the function more flexible and easier to reuse across environments. Hardcoding it means every change requires modifying and redeploying the code.
### 3. What does `list_objects_v2` return and how do you handle pagination if the bucket has more than 1000 objects?

list_objects_v2 returns a list of objects in the bucket along with metadata like size and key. If there are more than 1000 objects, pagination is handled using a continuation token to fetch the next set of results.
### 4. What is the difference between this task (scheduled Lambda) and Task 2.30 (event-driven Lambda)?

This task uses a scheduled Lambda that runs at fixed intervals, while Task 2.30 used an event-driven Lambda triggered automatically by S3 uploads. One is time-based execution and the other reacts to events
### 5. In a real DevOps scenario, when would you choose a scheduled Lambda over an event-driven one?

In real DevOps scenarios, scheduled Lambda is used for periodic tasks like monitoring, reporting, or cleanup jobs. Event-driven Lambda is used when actions need to happen immediately after an event occurs.
### 6. What happens if you forget to disable the EventBridge rule after testing?

If the EventBridge rule is not disabled after testing, the Lambda will keep running every minute and may incur unnecessary costs. It can also generate unwanted logs and executions.
### 7. Why must the EventBridge rule have permission to invoke Lambda, and where is that permission configured?

EventBridge must have permission to invoke Lambda because it is the service triggering the function. This permission is configured either in the Lambda resource-based policy or through an IAM role used by the scheduler.