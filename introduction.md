# Introduction

## About Terrable

Terrable is a tool for running and debugging AWS API Gateways (defined in
Terraform) locally.

It consists of two components:

- A Terraform module that provides a convenient way of deploying supported
  infrastructure to AWS
- A CLI tool that can understand and locally run the Terraform module

Terrable was inspired by Serverless Framework and some of its choice plugins.
However, Terrable treats Terraform as a first-class citizen.

## Why Terrable?

### No more CloudFormation

Serverless Framework is pretty awesome, but it ties you to CloudFormation and
its various limitations:

- Stack resource limits
- Slow deployments
- `UPDATE_ROLLBACK_FAILED` states

Terraform has none of these limitations: no resource limits, lightning-fast
deployments, and excellent state management.

### Native Terraform Integration

Because Terrable works with Terraform, you can use other Terraform resources
alongside the Terrable module. Need to reference resource ARNs or IDs in an IAM
policy? Simply reference them like any other Terraform resource! No hard-coding
or complex CloudFormation lookups needed.

### Free and Open Source

Serverless Framework V4 is no longer straightforwardly-free. If you just want to
generate and deploy some infrastructure, there's no reason this needs to be a
premium service.

### Lightweight Alternative to SST

SST is another alternative, but it requires an opinionated, heavy workflow.
While incredibly powerful, if you just want to run, debug and deploy some APIs,
it might be a relatively complex toolchain to adopt - particularly when working
with pre-existing infrastructure.

### Infrastructure as Code Consolidation

If you're using Terraform, Terrable lets you keep all your infrastructure as
code together. There's no need to:

- Juggle YAML files
- Spin up additional JavaScript CDK projects
- Maintain multiple infrastructure-as-code systems

With Terrable, you can write Terraform, and get a great local-development
experience out of the box.
