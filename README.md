# Todd's Notes

## Updating

Periodically, it will be wise to update the package versions specified in the package.json file. Do this by creating a local sam project from this cookie cutter using the sam init command below, and then manually updating the versions using

```sh
npm uninstall yada
npm install --save-dev yada
```

etc...

## Generating sample events

```sh
sam local generate-event apigateway aws-proxy | pbcopy
```

to see supported events

```sh
sam local generate-event -h
```

## Adding additional lambda functions

The following files must be modified to add additional lambda functions.

template.yml

```yaml
SecondFunction:
  Type: AWS::Serverless::Function
  Metadata:
    BuildMethod: makefile
  Properties:
    Handler: dist/handlers/second.secondHandler
    Description: A second function built on top of the same infrastructure.
    Events:
      Api:
        Type: Api
        Properties:
          Path: /second
          Method: GET
```

src/handlers/second.ts

```typescript
export const secondHandler = async ( ... ) { ... }
```

Makefile

```Makefile
build-SecondFunction:
	$(MAKE) HANDLER=src/handlers/second.ts build-lambda-common
```

# Template for AWS SAM using TypeScript and shared layers

This is a [Cookiecutter](https://github.com/audreyr/cookiecutter) template to create a Serverless application based on [Serverless Application Model (SAM)](https://aws.amazon.com/serverless/sam/) on Node.js 14 runtime using TypeScript for Lambda functions source code and shared layers for runtime dependencies.

Resulted application will be close to this example app: [aws-sam-typescript-layers-example](https://github.com/Envek/aws-sam-typescript-layers-example/)

## Usage

[Install AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html) and run following command:

```sh
sam init --location gh:Envek/cookiecutter-aws-sam-typescript-layers
```

You'll be prompted a few questions to help this template to scaffold this project and after its completion you should see a new folder at your current path with the name of the project you gave as input.

## License

This project is licensed under the terms of the [MIT license](https://opensource.org/licenses/MIT).
