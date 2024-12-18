# VPC assignment

## Assigning security groups and subnets

To run your handlers inside a VPC, you can use the `vpc` variable.

```terraform  hl_lines="4 5 6 7"
module "simple_api" {
  source = "terrable-dev/terrable-api/aws"
  api_name = "my-api"
  vpc = {
    subnet_ids         = ["subnet-12345"]
    security_group_ids = ["sg-67890", "sg-4567"]
  }
  handlers = {
    TestHandler: {
        source = "./TestHandlerSource.ts"
        http = {
          GET = "/"
        }
    }
  }
}
```

This will connect all your handlers to the specified subnets and security groups.

!!!note
  
    Specifying a VPC configuration may significantly slow down the _initial_ deployment
    of your service. With this setting, there's some additional networking configuration 
    that needs to be done on the AWS side.

    However, subsequent deployments should be much faster.