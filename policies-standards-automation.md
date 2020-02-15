# Policies and Standards Automation

- IAM
  - role trust policy
  - user must be given a policy that allows it to assume a role
  - conditions like IP whitelist or MFA required
- encryption in transit and rest
  - if end-to-end required
    - either terminate TLS at EC2
    - or at ALB and re-encrypt to HTTPS backend
  - S3 at rest
    - default encryption best
    - SSE-S3, SSE-KMS with either AWS managed CMK or customer managed CMK
    - Glacier is only storage service that encrypts by default
- managed security services
  - "network intrusion detection"
    - should think of GuardDuty
    - analyzes CloudTrail events, VPC flow logs, DNS query logs
  - Amazon Inspector
    - install agent on EC2
    - automated host assessments
    - can select rule sets (security best practices, CVE, host hardening, etc.)
- Systems manager
  - supports on-premise/cloud and windows/linux
  - run command: script or commands across a whole fleet of resources, no sign in necessary
  - session manager
  - patch manager: use patch baseline to install patches with attestation
    - maintenance windows of when to apply patches
  - parameter store: contain config information and secrets and encrypt with KMS
    - secrets manager also does rotation
  - license manager: audit what instances are using licenses
- AWS Config
  - trigger or time based evaluation of resources
  - maintain history of compliance
- cost optimization
  - always keep in mind even if question doesn't call it out specifically
  - trusted advisor
  - tagging important
- service catalog
  - good way to enforce compliance and security
- EC2 instance compliance (secure, automated, auditable ways to configure your instances)
  - golden images/pre-baked AMIs
  - user data scripts
  - configuration management (chef/puppet/ansible/OpsWorks/SSM)