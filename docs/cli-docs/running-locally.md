# Running Locally

Terrable modules can be run locally via the CLI via the `terrable offline` command.

The file name and terraform module name need to be passed to the command using the `--file` and `--module` flags.
For example:

```bash
terrable offline --file "file.tf" --module "module_name"
```

You can also specify a `--port` option if you want the local server to start listening on a specific port.

## Setting it up

Assuming you've installed the Terrable CLI, let's start by writing some TypeScript for our 
endpoint. Create a new file, and we'll call it `TestHandlerSource.ts`.

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

Let's also create a Terraform file called `my_api.tf`, and in it we'll add the terrable-api module:

```Terraform
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

Here, we can see that we've configured a handler called `TestHandler`, that uses the `TestHandlerSource.ts` file
we've just written as its source. It also specifies a `GET` endpoint with the path `/`.

Let's run our Terrable API locally. From the same directory you've created these files in:

```bash
terrable offline -f "my_api.tf" -m "simple_api"
```

You'll see an output similar to the following:

```bash
1 Endpoint(s) to prepare...
   GET   http://localhost:8080/
Starting server on :8080
```

Now, making a GET request to `http://localhost:8080/` should execute our handler code, and return
the following JSON response:

```bash
200 OK
{
  "message": "Hello world!"
}
```