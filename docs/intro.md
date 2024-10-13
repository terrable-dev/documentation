# Introduction

Terrable is a tool for running and debugging AWS API Gateways (defined in Terraform) locally.

Terrable is made up of two components: a Terraform module for deploying API Gateways to AWS
and a companion CLI tool that can understand and locally run an instance of your configuration for that module.

Terrable was inspired by serverless framework and some of its choice plugins. However, Terraform is a first-class
citizen in this world. There's not a slow, unreliable CloudFormation stack in sight!

## Why

Serverless framework is pretty awesome, but you're stuck with CloudFormation and its various limitations
by using it. There are no resource limits in Terraform - "just break it down into microservices" might be some pretty 
inconvenient advice.

By providing a Terraform module, we're also able to use other Terraform resources alongside our module.

There are also situations where you want to dynamically reference the ARN or Id of various resources, such as in an IAM policy. 
Well, now it's just Terraform. Just reference it like any other Terraform
resource! No need to do any hard-coding or funky CloudFormation resource lookups.

SST is another alternative, but (currently) it requires a pretty opinionated workflow. It's incredibly
cool, but if you just want to debug some APIs, it's a relatively complex toolchain to get into. 

Terrable also gives you the option to keep all your infrastructure as code together, assuming you want to use Terraform.
There's no need to juggle YAML files or to spin up some additional JavaScript CDK projects to have a workable local-development experience.
