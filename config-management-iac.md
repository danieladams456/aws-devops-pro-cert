# Configuration Management and Infrastructure as Code

## CloudFormation

- template sections
  - parameters
  - mappings
  - conditions
  - resources
  - outputs
- [how to use change sets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html)
- [how CF handles updates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-update-behaviors.html)
  - updates with no interruption: cloudwatch metric tweak
  - updates with some interruption: EC2 instance resize
  - replacement: new physical ID, i.e. RDS MySQL to Postgres
- how to use update policies
  - MaxBatchSize
  - MinInstancesInService
  - MinSuccessfulInstancesPresent
  - PauseTime
  - KNOW ALL SETTINGS!
  - WaitOnResourceSignals: wait for required number of success signals
  - SuspendProcesses: prevents AutoScaling from interfering with the stack update
- layering with nested stacks
  - chaining and nesting?
- DependsOn
  - explicit vs implicit
  - simple launch
  - WaitCondition/WaitConditionHandle
  - CreationPolicy
    - (part of EC2 and AutoScaling, not a standalone resource)
    - know the difference between WaitCondition and CreationPolicy
- stack policies
  - protect how and what is updated in a stack
  - open by default, but if stack policy present then default switches to deny
- custom resource workflow
  - token and response
- StackSets
  - management of duplicate stacks across regions and accounts
- helper scripts
  - `cfn-init`: executes CloudFormation metadata one time, typically called in user data
  - `cfn-hup`: monitors CloudFormation metadata and applies changes when discovered
  - `cfn-signal`: provides a completion signal to designate the creation (successful or not) of a CreationPolicy or WaitCondition
  - `cfn-get-metadata`: view current metadata stored within the CF stack (not much exam coverage)
- policy types
  - update policies
    - `AutoScalingRollingUpdate`
    - `AutoScalingReplacingUpdate`
    - others?
  - stack policies
  - creation policies
  - deletion policies
- deploying with CloudFormation
  - deploy with CI/CD pipeline
  - can create pipeline, OpsWorks, or Elastic Beanstalk

## Elastic Beanstalk

- environment
  - tiers: web and worker
  - types: single instance and autoscaling
  - application is the top level construct, can have multiple environments
- application versions
  - application code stored i`n S3
  - deploy different versions to different environments
  - support for rollbacks and rolling updates
- configuration
  - yaml `*.config` files in `.ebextensions` folder
  - can install additional yum packages
- updates
  - in-place
  - environment swap: **look at eb cli**
    - `eb clone`
    - `eb deploy`
    - `eb swap`
  - also supports ECS for multi-container Docker environments
- ## `.ebextentions` [configuration options](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/ebextensions.html)

## OpsWorks

![OpsWorks offerings](./pictures/opsworks-offerings.png)

- app infrastructure management via either puppet or Chef
- use layers for each tier (load balancer, app server, database layer)
- know permissions structure
- know lifecycle: setup/configure, deploy, undeploy, shutdown (Chef specific?)
- chef recipes
  - executed at each stage of the lifecycle
  - recipe + metadata can run a command

## Containers

- ECS/EKS with EC2/Fargate
- ECR for images
- CodeCommit -> CodePipeline: CodeBuild for image build, CloudFormation for deploy

## Serverless Computing

- Lambda
- how to trigger
- API Gateway
  - host multiple versions/stages
  - API keys
  - VPC integration

## Exam Tips

- **know CloudFormation in depth**
- multiple high level services (Fargate, Elastic Beanstalk, OpsWorks): know the level/area of control for each and when you would want to use one or the other
- CI/CD for Docker applications on ECS
- routing "portions of users" will always be Route53
