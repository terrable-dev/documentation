# Introduction

## About Terrable

Terrable is a tool for running and debugging AWS API Gateways (defined in Terraform) locally.

Terrable is made up of two components: a Terraform module for deploying API Gateways to AWS
and a companion CLI tool that can understand and locally run an instance of your configuration for that module.

Terrable was inspired by Serverless Framework and some of its choice plugins. However, Terraform is a first-class
citizen in this world. There's not a slow, unreliable CloudFormation stack in sight!

## Why

Serverless Framework is pretty awesome, but you're stuck with CloudFormation and its various limitations
by using it. Stack resource limits, slow deployments, `UPDATE_ROLLBACK_FAILED`, to name a few. There are no resource limits in Terraform,
it's lightning fast when deploying changes, and the state management is excellent.

By using the Terrable Terraform module, you're also able to use other Terraform resources alongside the Terrable module. 
There are situations where you want to dynamically reference the ARN or Id of resources, such as in an IAM policy. 
Well, now it's just Terraform! Simply reference it like any other Terraform
resource! No need to do any hard-coding or funky CloudFormation resource lookups.

SST is another alternative, but (currently) it requires a pretty opinionated workflow. It's incredibly
cool, but if you just want to run, debug and deploy some APIs, it's a relatively complex toolchain to get into. 

Terrable also gives you the option to keep all your infrastructure as code together, assuming you want to use Terraform.
There's no need to juggle YAML files or to spin up some additional JavaScript CDK projects to have a workable local-development experience.
