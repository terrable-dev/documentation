# API gateway

By default, Terrable will create [HTTP API Gateways (V2)](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api.html).

However, it is possible to create [REST API Gateways (V1)](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-rest-api.html)
by specifying explicitly the `http_api` or `rest_api` parameters.

You can refer to the [AWS documentation](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-vs-rest.html) to decide which one best suits your needs.

You can use the following syntax to define either an HTTP API or a REST API.

=== "HTTP API (V2)"

    ```terraform hl_lines="4 5"
    module "simple_api" {
      source = "terrable-dev/terrable-api/aws"
      api_name = "my-api"
      http_api = {
      }
      handlers = {
        TestHandler: {
            source = "./TestHandlerSource.ts"
            http = {
              GET   = "/route"
            }
        }
      }
    }
    ```

    !!! tip

        The `http_api` parameter is optional if you want an HTTP API but don't need any specific configuration, such as a custom domain.

=== "REST API (V1)"

    ```terraform hl_lines="4 5"
    module "simple_api" {
      source = "terrable-dev/terrable-api/aws"
      api_name = "my-api"
      rest_api = {
      }
      handlers = {
        TestHandler: {
            source = "./TestHandlerSource.ts"
            http = {
              GET   = "/route"
            }
        }
      }
    }
    ```

## Limitations

### Stages 

Terrable does not support stages in API Gateway, and instead substitutes a `default` stage where
applicable. This has some upsides: You typically don't need to worry about deploying a stage, as
terrable will do this for you. It also simplifies the usage of the module.

If you depend on stages then Terrable probably isn't right for you at this moment in time.

If you want to deploy multiple versions of the module (i.e. for production, staging, test environments)
then approach it as you would any other Terraform module in your infrastructure.