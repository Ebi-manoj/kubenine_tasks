
### 1. What is the difference between fixed capacity and dynamic scaling?

Fixed capacity keeps a constant number of instances regardless of traffic, so it doesn’t adapt to load changes. Dynamic scaling automatically adjusts the number of instances based on demand, improving both performance and cost efficiency
### 2. What is target tracking scaling and how does it decide when to scale?

Target tracking scaling maintains a chosen metric like CPU at 50% by continuously monitoring it using CloudWatch. If the metric goes above or below this target, it automatically adds or removes instances to bring it back to the desired level.
### 3. What is a cooldown period and why does it exist?

A cooldown period is a short waiting time after a scaling action before another one can occur. It exists to prevent scale flapping.
### 4. What happens if the scaling threshold is set too low?

If the threshold is too low, scaling triggers too early and too often, leading to unnecessary instance launches. This increases cost without providing real performance benefits.
### 5. What is the difference between scale-out and scale-in?

Scale-out means adding more instances when demand increases to handle traffic. Scale-in means removing extra instances when demand drops to reduce cost.
### 6. How does CloudWatch trigger Auto Scaling actions?

It collects metrics like CPU usage and evaluates them against defined thresholds or targets. When conditions are met, it signals the Auto Scaling Group to adjust capacity.
### 7. How does dynamic scaling directly affect AWS cost?

Dynamic scaling helps reduce cost by running fewer instances during low demand and more during high demand. We only pay for the capacity you actually need at any given time
### 8. What is scaling flapping and how do you prevent it?

Scaling flapping is when instances are repeatedly launched and terminated due to frequent metric changes. It’s prevented using cooldown periods, proper threshold tuning.