# Writing handlers

Currently, Terrable supports JavaScript and TypeScript handlers.

Your handler functions should export a function called `handler`, in order to be interpreted correctly
by Terrable.

The following code demonstrates a basic handler that, when invoked, will return
a JSON body with the message 'Hello world!'

```TypeScript
const handler = async (event) => {
    return {
        statusCode: 200,
        headers: {
            "Content-Type": "application/json",
        },
        body: JSON.stringify({
            message: 'Hello world!',
        }),
    }
}

export { handler };
```

This handler can then be referenced in the Terrable Terraform module as such:

```terraform
module "simple_api" {
  source = "terrable-dev/terrable-api/aws"
  api_name = "my-api"

  handlers = {
    TestHandler: {
        source = "./TestHandlerSource.ts"
        http = {
          GET = "/",
        }
    },
  }
}
```