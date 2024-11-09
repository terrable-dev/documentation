## Limitations

### Stages 

Terrable does not support stages in API Gateway, and instead substitutes a `default` stage where
applicable. This has some upsides: You typically don't need to worry about deploying a stage, as
terrable will do this for you. It also simplifies the usage of the module.

If you depend on stages or just prefer to work with them then Terrable probably isn't right for you at this moment in time.

If you want to deploy multiple versions of the Terrable API module (i.e. for production, staging, test environments)
then approach it as you would any other Terraform module in your infrastructure.