# Assigning policies

## Global and individual policies

You can assign policies to individual handlers, and also globally at the service level.

## Global policies

To set global policies that will be applied to every handler, the `global_policies` input can
be used.

```terraform hl_lines="4 5 6 7 8"
module "simple_api" {
  source = "terrable-dev/terrable-api/aws"
  api_name = "my-api"
  global_policies = {
    # apply these policies to all handlers
    GlobalPolicy1 = "arn:aws:iam::aws:policy/GlobalPolicy1"
    GlobalPolicy2 = "arn:aws:iam::aws:policy/GlobalPolicy1"
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

This will assign `arn:aws:iam::aws:policy/GlobalPolicy1` and `arn:aws:iam::aws:policy/GlobalPolicy1`
to every handler in your configuration.

## Individual handler policies

To specify policies for individual handlers, the `policies` attribute can be set when
declaring a handler.

```terraform hl_lines="6 7 8"
module "simple_api" {
  source = "terrable-dev/terrable-api/aws"
  api_name = "my-api"
  handlers = {
    TestHandler: {
        policies = {
          HandlerOnePolicy = "arn:aws:iam::aws:policy/HandlerOne" # will only be applied to 'TestHandler' 
        }
        source = "./TestHandlerSource.ts"
        http = {
          GET = "/"
        }
    },
    NoPolicyHandler: {
        source = "./TestHandlerSource.ts"
        http = {
          GET = "/"
        }
    }
  }
}
```

This will apply the `arn:aws:iam::aws:policy/HandlerOne` policy to the `TestHandler` handler.
However, the will not be applied to any other handlers, such as `NoPolicyHandler` in the above example.

