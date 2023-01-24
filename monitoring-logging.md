# Monitoring and Logging

## CloudWatch

- logs, metrics, alarms, events, rules, targets
- Metrics
  - ELB metrics can be used for scaling besides just host metrics
    - `SurgeQueueLength`: backend can't process requests as quickly as they are being received
    - `SpilloverCount`: requests dropped
  - know how to use custom metrics
- Logs
  - also know VPC flow logs and ELB logs
  - test topics
    - how to use CloudWatch agent to send logs
    - analyze logs via filter
    - update a metric based on occurrences
    - send a notification via SNS
- Alarms
  - notification and automated action based on a threshold
- Events
  - react to changes in the environment
  - CWE Rules match events to Targets
  - Targets receive payload as JSON and process it

## Additional Logging/Monitoring

- CloudTrail
  - AWS API activity and Console login recorder
- Xray
  - application level monitor
  - tracing API calls
- Kinesis
  - data streams: scalable ingestion
  - firehose: load directly to storage (S3, ES)
  - analytics: process via SQL

## Monitoring CI/CD

- CodeCommit
  - Events for commits, branch creation or deletion
  - SNS for commits (verify exact constraints)
- CodeBuild
  - Events for build state changes
- CodeDeploy
  - SNS triggers before, during, and after deploy
- CodePipeline
  - Events for pipeline, stage, or action status change

## Tagging

- metadata for resources
- enforce through automation

## Exam Tips

- know CloudWatch in-depth, data kept for 15 months
- know the various logging options and which is cost effective/efficient for a scenario
- questions about reporting API calls will most likely be CloudTrail
- tagging for resource categorization
