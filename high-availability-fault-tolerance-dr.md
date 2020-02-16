# High Availability, Fault Tolerance, and Disaster Recovery

## High Availability

- use multiple AZs and based on requirements potentially multi-region
- ASGs can keep warm standby cost efficient

## Scalability

- (looking at other functionality besides just ASGs)
- Lambda@Edge + CloudFront caching
- SQS
  - decouple for availabilityand
  - absorb burst traffic
  - fan out with SQS
- data storage scaling
  - don't need to worry about S3
  - EBS has PIOPS
  - RDS scaling: use read replicas or caching
  - DynamoDB: adjust WCU/RCU

## Disaster Recovery

- RPO: recovery point objective
  - potential for data loss if a failure happens
- RTO: recovery time objective
  - threshold of downtime that the system can allow
- patterns
  - backup/restore: low priority apps just save off data, RTO/RPO hours
  - pilot light: core services have to scale up to handle traffic, RTO/RPO minutes
  - warm standby
    - "little bit more" compute than pilot light
    - business critical services, RTO/RPO seconds
    - can also allow for A/B or canary testing
  - hot standby: fully scaled out active/active, RTO/RPO instant

## Test Hints

- create applications that are resilient to failures
- know how to decouple your infrastructure
- know how to move elastic IPs/elastic network interfaces between resources for failover
- know how ELBs/ASGs work together
