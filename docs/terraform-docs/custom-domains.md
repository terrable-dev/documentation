# Custom domains

If you wish to connect a custom domain to your API, you can use the `http_api` input.

```terraform
module "simple_api" {
  source = "terrable-dev/terrable-api/aws"
  api_name = "my-api"
  http_api = {
    custom_domain   = "testdomain.test.com"
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

Terrable will attempt to create a custom domain (`testdomain.test.com`) and map it to your API.
It will also try to create an TLS certificate.

!!! note

    The base domain must exist on the AWS account being targeted. 

    The Terrable module will attempt to find the base domain in the Route53 zones of the AWS account.
    In the example above, if `test.com` isn't a hosted zone, it will fail.

## Use an existing ACM certificate

If you already have an ACM certificate configured, you can pass it in alongside your `custom_domain`:

```terraform
module "simple_api" {
  source = "terrable-dev/terrable-api/aws"
  api_name = "my-api"
  http_api = {
    custom_domain   = "testdomain.test.com"
    certificate_arn = "arn:aws:acm:us-east-1:123456789012:certificate/existing-cert-id"
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
