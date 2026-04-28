
### 1. What is the difference between target tracking scaling and step scaling?

Target tracking tries to maintain a specific metric value like CPU at 50% and automatically adjusts tasks. Step scaling uses predefined rules, giving us more manual control.
### 2. Why are minimum and maximum task limits important for auto scaling?
Minimum ensures our app never scales down to zero and stays available. Maximum prevents over scaling, protecting us from unexpected high costs or resource exhaustion
### 3. What CloudWatch metrics are commonly used to scale ECS services?

CPUUtilization and MemoryUtilization.
### 4. What is a cooldown period and what problem does it solve?

A cooldown is a waiting period after scaling before another action can happen. It prevents rapid scaling up and down due to temporary spikes or fluctuations.
### 5. What happens when ECS launches a new task — how does traffic reach it?

When a new task starts, it registers with the target group attached to the load balancer. Once it passes health checks, the load balancer begins routing traffic to it automatically.
### 6. If CPU stays at 15% and your threshold is 20%, will scaling happen? What if it briefly spikes to 25% and drops back?

At 15%, scaling won’t happen because it’s below the threshold. A brief spike to 25% usually won’t trigger scaling unless it stays high long enough to be considered a sustained increase.
### 7. Why might you want different cooldown periods for scale-out vs scale-in?

Scale-out cooldown is shorter so the system can respond quickly to rising load. Scale-in cooldown is longer to avoid removing capacity too early when traffic might return.
### 8. When you update a service's task definition, how does ECS replace the running tasks?

ECS performs a rolling update by starting new tasks with the updated definition first. Once they’re healthy, it gradually stops the old tasks, ensuring no downtime.