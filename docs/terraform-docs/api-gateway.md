# API gateway

## HTTP vs REST APIs

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

## Endpoint Types

### Regional

By default, Terrable will configure REST API Gateways (V1) with the `REGIONAL` endpoint type.

HTTP API Gateways (V2) only support `REGIONAL` [endpoint types](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-vs-rest.html).

### Private

When using REST API Gateways (V1), the `PRIVATE` endpoint can be configured via the Terrable module using the following options:

- `rest_api.endpoint_type` can be set to `PRIVATE`
- `rest_api.vpc_endpoint_ids` takes an array of VPC Endpoints that should be able allowed to invoke the API Gateway.

```terraform hl_lines="4 5 6 7"
module "simple_api" {
  source = "terrable-dev/terrable-api/aws"
  api_name = "my-api"
  rest_api = {
    endpoint_type    = "PRIVATE"    # Configures the API endpoints as 'PRIVATE'
    vpc_endpoint_ids = ["vpce-id"]  # The VPC Endpoints to be used to generate the resource policy
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

This configuration will create a REST API that can only be invoked from the provided VPC Endpoints.

This allows you to secure an API Gateway to your VPC.