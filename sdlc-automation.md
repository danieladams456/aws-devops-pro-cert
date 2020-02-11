# Domain 1 - SDLC Automation

_22% of exam_ (largest domain)

## Pipeline: source, build, test, deploy, monitor

- Continuous Integration: through build and basic test
- Continuous Delivery: more testing and deployment with a human in the loop
- Continuous Deployment: taking the human out, automated monitoring and potential rollback post deploy
  - through production
  - "quick, safe, and automated"
- Automation should be in every answer
  - only time for a manual step is in continuous delivery approval phase
  - immutable means you can only deploy code changes through the pipeline

## AWS Code Services

- CodeCommit (source)
  - highly available, preferable to managing yourself
  - branch for features and debugging, merge back to master quickly
  - notifications via CloudWatch Events to trigger CodeBuild
- CodeBuild (build and test)
  - fully managed build service, Jenkins replacement
  - `buildspec.yml` structure
  - how it integrates with other services (CloudWatch events, artifact handoff)
- CodeDeploy (deploy)
  - can deploy to EC2 via agent, ECS via target groups, Lambda via aliases
  - only deploys the app, does not do system configuration
  - can handle deployment verifications for blue/green
  - creates a `revision`
    - source code + `appspec.yml`
    - artifacts can be hosted on S3 or CodeCommit
  - deployment types (looks like EC2 options?)
    - OneAtATime
    - HalfAtATime
    - AllAtOnce
- CodPipeline (tie it all together)
  - orchestration of the Code suite actions and transitions
  - compatible with Continuous Delivery with manual approval step

## Testing

- Pyramid: Unit (most), Integration, Performance/Compliance, UI (least)
  - exam questions pick options with good unit test coverage
  - also test infrastructure: performance and security vulnerability
- TDD: test driven development
  - unit test fail fast and prevent deploys of bad code earlier

## Deployment Strategies

- in-place: take compute offline, install new code all at once, bring back up
- rolling: staggered versions of in-place deployments, functioning in degraded state for a short period of time but not complete outage
- canary: test out new code/configuration with real world traffic
- blue/green: **most covered topic on exam**, bring up new environment, swap to it, validate new works, fail back if necessary
- immutable: when rolling out new code, don't modify in place. Bring up new instances

## Test Axioms

- have a firm understanding on each AWS Code service
- be able to apply updates in your environment and recover from any issues
- know each of the deployment strategies, how to implement, and how to troubleshoot any issues
- remember to test everything
