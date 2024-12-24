# Quick Start

This quick start will show you how to get a minimal Terrable configuration setup
as quickly as possible.

We'll create an API endpoint written in TypeScript, and we'll also write some
Terraform that Terrable can understand.

### Project Structure

Terrable works by connecting two key components:

- A Terraform module that defines your infrastructure
- The Terrable CLI tool that runs your API locally for development

Your project structure should look something like this:

```
your-project/
├── TestHandler.ts
├── simple_api.tf
```

### Install Terrable

Firstly, make sure you've [installed](/installation) the Terrable CLI.

### Create a handler

Create a file called `TestHandler.ts` in your project directory:

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

### Create an API Definition

Then, in your terraform project (or just as a standalone file for testing
purposes) let's add a `simple_api.tf` Terraform file:

```terraform
module "simple_api" {
  source = "terrable-dev/terrable-api/aws"
  api_name = "my-api-name"

  handlers = {
    TestHandler: {
        source = "./TestHandler.ts" # Path is relative to your 'simple_api.tf' Terraform file
        http = {
          GET = "/"
        }
    }
  }
}
```

### Run Terrable

Using the installed `terrable` CLI, you can run this configuration locally in
the following way:

```shell
terraform offline -f simple_api.tf -m simple_api
```

You should see Terrable spin up a local server and provide an output similar to:

```shell
Starting terrable local server...
1 Endpoint to prepare...

 GET  http://localhost:8080/  (TestHandler) 

Server started on :8080
```

You can now make a HTTP request to that endpoint to get the 'Hello world!'
response:

```shell
curl http://localhost:8080/
```

Which returns:

```shell
{"message":"Hello world!"}
```

### How It Works

When you run the Terrable CLI (terrable dev), it:

- Reads your Terraform configuration
- Identifies the Terrable API module
- Sets up a local development server that mirrors how your infrastructure would
  behave in AWS
- Watches your TypeScript files for changes and automatically reloads the server

The Terraform module serves two purposes:

- In development: It provides the CLI with information about your API structure,
  allowing Terrable to emulate certain behaviours locally
- In production: It creates the actual AWS infrastructure (Lambda functions, API
  Gateway, etc.)
