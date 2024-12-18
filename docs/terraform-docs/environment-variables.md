# Environment variables

## Global and individual environment variables

You can set environment variables for your service at a global level, or at the individual
handler level.

## Global environment variables

You can set the `global_environment_variables` input to specify global environment variables:

```terraform hl_lines="4 5 6 7"
module "simple_api" {
  source = "terrable-dev/terrable-api/aws"
  api_name = "my-api"
  global_environment_variables = {
    # will provide these variables to all handlers
    GLOBAL_ONE = "global-value"
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

This will set the env var `GLOBAL_ONE=global-value` in all of your deployed handlers.

!!!tip

    Remember, this is just Terraform! You can reference other resources and data sources in 
    your environment variable declarations!

    Try something like:

    ```terraform hl_lines="1 2 3 9"
    resource "aws_sqs_queue" "sqs_queue" {
      name = "my-sqs-queue"
    }

    module "simple_api" {
      source = "terrable-dev/terrable-api/aws"
      api_name = "my-api"
      global_environment_variables = {
        SQS_QUEUE_URL = aws_sqs_queue.sqs_queue.url
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

    To set a global `SQS_QUEUE_URL` variable that's set to the value of the Terraformed queue's URL.

## Handler specific environment variables

You can set environment variables for individual handlers by passing an `environment_variables`
value into your handler declarations:

```terraform hl_lines="6 7 8 9"
module "simple_api" {
  source = "terrable-dev/terrable-api/aws"
  api_name = "my-api"
  handlers = {
    TestHandler: {
        environment_variables = {
          # will provide these variables to only 'TestHandSler'
          LOCAL_HANDLER_ONE = "local-value"
        }
        source = "./TestHandlerSource.ts"
        http = {
          GET = "/"
        }
    }
  }
}
```

This will set a `LOCAL_HANDLER_ONE=local-value` environment variable just for `TestHandler`.
Any global variable declarations will be merged with any local ones.
