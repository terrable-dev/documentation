# Minimal Terraform configuration

## Minimal config example

A minimal Terrable Terraform configuration may look something like the following:

```terraform
module "simple_api" {
  source = "terrable-dev/terrable-api/aws"
  api_name = "my-api"
  
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

We define a module (`simple_api`) that the Terrable CLI can look for, an API name, and a handler
with a HTTP endpoint (`TestHandler`).

