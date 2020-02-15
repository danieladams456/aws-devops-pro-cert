# Incident and Event Response

## Troubleshooting Tips

- logging strategy
  - centralized for easy access
  - keep them for long term analysis
  - _securely_ log anything that would be helpful
- AWS X-Ray
  - identify performance bottlenecks in a complex system
- example scenario:
  - email alert for root access key usage: CloudTrail -> CloudWatch logs -> metric filter -> CloudWatch alarm -> SNS
  - cfn-init vs cloud-init

## AutoScaling

![autoscaling lifecycle](./pictures/auto-scaling-lifecycle.png)

- [suspending processes](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-suspend-resume-processes.html)
  - know which processes can be suspended
    - `Launch`: disrupts other processes
    - `Terminate`: disrupts other processes
    - `AlarmNotifications`: accepts notifications from CloudWatch alarms that are associated with the group
    - `ReplaceUnhealthy`
      - terminates instances that are marked unhealthy and later creates instances to replace them
      - lets you sign in to unhealthy instances to troubleshoot
    - `HealthCheck`: checks health of instances
    - `AZRebalance`: balances the number of EC2 instances across AZs within the region
    - `AddToLoadBalancer`
      - adds instances to the load balancer when they are launched
      - suspending this process and re-enabling will require you to manually attach instances launched during the time it was suspended
  - know how to suspend processes (console/cli)
  - know why you would suspend certain processes/what is affected
- [standby state](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-enter-exit-standby.html)
  - ASG removes instance from load balancer or target group
  - stops sending health checks
  - you can do any maintenance or troubleshooting during this time
  - when entering standby state, ASG by default decrements ASG size to not launch a new one
    - you can specify not to do this and launch a replacement if you need that capacity
  - when returning to service:
    - if you used the default and decremented desired count, it is incremented and nothing happens
    - if you specified not to decrement, the instance is returned but count remains unchanged. Now you are over count and an instance is terminated according to the termination policy
- [lifecycle hooks](https://docs.aws.amazon.com/autoscaling/ec2/userguide/lifecycle-hooks.html)
  - puts the instance in `Pending:Wait` or `Terminating:Wait` states until the hook completes via callback or timeout reached (default 1 hour)
  - notification types
    - Lambda via CloudWatch event (callback token)
    - SNS (callback token)
    - SQS (callback token)
    - script can be run in instance user-data on startup and then call back with instance ID to complete the hook
- [termination policies](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-instance-termination.html)
  - Default
    1. chooses AZ with most instances to maintain balance
    2. choose the lower priority type of using allocation strategies for spot vs on-demand
    3. oldest launch template or configuration
       1. oldest launch config first
       2. any launch config if exists (even if another launch template is older)
       3. oldest launch template
    4. closest to next billing hour (per-second billing only applies to Amazon Linux and Ubuntu)
  - Custom
    - if one AZ has more instances policy is applied just within that AZ, otherwise across all AZs
    - OldestInstance, NewestInstance, OldestLaunchConfiguration, OldestLaunchTemplate, AllocationStrategy, ClosestToNextInstanceHour, Default
  - scale-in protection
    - can be applied to individual instances in an ASG or the group as a whole
    - ASG will not terminate them for scale in reasons
    - still can be terminated by
      - EC2 manual termination
      - health check failure (different from Standby in this sense and also that it is still receiving traffic)
      - Spot interruptions
