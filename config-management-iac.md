# Configuration Management and Infrastructure as Code

## CloudFormation

- template sections
  - parameters
    - pseudo parameters: AWS::AccountId, AWS::Region, AWS::StackId, etc
  - mappings
  - conditions
  - resources
  - outputs
- [how to use change sets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html)
  - when an stack update is run, all change sets are deleted as they are no longer valid
- [how CF handles updates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-update-behaviors.html)
  - updates with no interruption: cloudwatch metric tweak
  - updates with some interruption: EC2 instance resize
  - replacement: new physical ID, i.e. RDS MySQL to Postgres
- DependsOn
  - explicit vs implicit
  - WaitCondition/WaitConditionHandle
  - CreationPolicy
    - (part of EC2 and AutoScaling, not a standalone resource)
    - know the difference between WaitCondition and CreationPolicy
- stack policies
  - protect how and what is updated in a stack
  - IAM policy format
  - open by default, but if stack policy present then default switches to deny
- custom resource workflow
  - either invokes Lambda or publishes SNS
  - response token used to put result
- StackSets
  - management of duplicate stacks across regions and accounts (just now AWS organization wide)
- helper scripts
  - [`cfn-init`](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-init.html): executes CloudFormation metadata one time, typically called in user data
  - [`cfn-hup`](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-hup.html): monitors CloudFormation metadata and applies changes when discovered
    - `cfn-hup.conf` file lets you specify the refresh interval, default 15 minutes
    - `hooks.conf` file or `hooks.d` directory lets you trigger local commands when the resource lifecycle hits certain stages
  - [`cfn-signal`](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-signal.html): provides a completion signal to designate the creation (successful or not) of a CreationPolicy or WaitCondition
  - [`cfn-get-metadata`](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-get-metadata.html): view current metadata stored within the CF stack (not much exam coverage)
- [EC2 initialization metadata](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-init.html)
  - utilized by `cfn-init` helper script
  - sections: packages, groups, users, sources, files, commands, and then services
- **policy types**
  - update policies
    - `AutoScalingRollingUpdate`
    - `AutoScalingReplacingUpdate`
    - others?
  - how to use update policies
    - MaxBatchSize
    - MinInstancesInService
    - MinSuccessfulInstancesPresent
    - PauseTime
    - KNOW ALL SETTINGS!
    - WaitOnResourceSignals: wait for required number of success signals
    - SuspendProcesses: prevents AutoScaling from interfering with the stack update
    - stack policies
    - creation policies
    - deletion policies
- deploying with CloudFormation
  - deploy with CI/CD pipeline
  - can create pipeline, OpsWorks, or Elastic Beanstalk
- security
  - [audit logging with CloudTrail](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-api-logging-cloudtrail.html)
    - CloudWatch events generic [rule](https://docs.aws.amazon.com/es_es/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-CloudTrail-Rule.html) for API calls via CloudTrail
    - could potentially be filtered more to get API calls done by the service CloudFormation
  - CloudFormation by default uses temporary credentials generated off of the user session, but can also be configured to use a [service role](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-iam-servicerole.html) instead

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
