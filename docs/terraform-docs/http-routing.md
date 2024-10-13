# HTTP routing

You can declare one or more HTTP routes for your handlers.

```terraform
module "simple_api" {
  source = "terrable-dev/terrable-api/aws"
  api_name = "my-api"
  handlers = {
    TestHandler: {
        source = "./TestHandlerSource.ts"
        http = {
          GET   = "/route"
          POST  = "/route"
        }
    }
  }
}
```

This configuration will allow you to invoke the `TestHandler` with **both** a `GET` and a `POST`
request to the `/route` endpoint.

## Path parameters

Variable path parameters can be specified with the `{curly}` syntax.

For example:

```terraform
module "simple_api" {
  source = "terrable-dev/terrable-api/aws"
  api_name = "my-api"
  handlers = {
    TestHandler: {
        source = "./TestHandlerSource.ts"
        http = {
          GET = "/users/{id}"
        }
    }
  }
}
```

This will dynamically match anything specified in place of `{id}` (eg. `/users/100`), and the value
of `id` (which would be `100`) will be passed to your handler's code.

## Allowed methods

All of the methods allowed by AWS API gateway are allowed. These include:

  - `GET`
  - `POST`
  - `PUT`
  - `DELETE`
  - `PATCH`
  - `HEAD`
  - `OPTIONS`
  - `ANY` (matches all of the above)