# aws-sam-lambda-example

### First commit WooSung-Jung  in 2020-12-18

## Install AWS SAM CLI

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
brew --version

brew tap aws/tap
brew install aws-sam-cli

sam --version
```

- If you use Linux or Windows

> https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install-mac.html
> 
> https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install-windows.html
> 
> https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install-linux.html

## Install Docker

![image1.jpg](./images/images1.png)

>https://hub.docker.com/ 

## Start AWS SAM CLI with Docker

### Step 1 - Download a sample application
```bash
sam init
```

### Step 2 - Choice  a sample application
```bash
1 - AWS Quick Start Templates
1 - Zip (artifact is a zip uploaded to S3)
9 - python3.6
<YOUR PROJECT NAME>
1 - Hello World Example
```

### Step 3 - Make docker image folder
```bash
cd <YOUR PROJECT NAME> # Example : cd aws-sam-lambda/
mkdir docker
cp -r hello_world/* docker
```

### Step 4 - Modify CodeUrl in template.yaml
```yaml
  HelloWorldFunction:
    Type: AWS::Serverless::Function # More info about Function Resource: https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#awsserverlessfunction
    Properties:
      CodeUri: hello_world_function
      Handler: hello_world/app.lambda_handler
      Runtime: python3.6
```

```yaml
  HelloWorldFunction:
    Type: AWS::Serverless::Function # More info about Function Resource: https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#awsserverlessfunction
    Properties:
      CodeUri: docker
      Handler: hello_world/app.lambda_handler
      Runtime: python3.6
```

### Step 5 - Build your application with Docker container
```bash
sam build --use-container
```
### Step 6 - Invoke your application with Docker container
```bash
sam local invoke
# TIP : sam build --use-container && sam local invoke
```

- Check result

```bash
Mounting /Users/XXX/Project/aws-sam-lambda-example/aws-sam-lambda/.aws-sam/build/HelloWorldFunction as /var/task:ro,delegated inside runtime container
START RequestId: 8393e7be-6a67-4b8d-b49c-45edaf928150 Version: $LATEST
END RequestId: 8393e7be-6a67-4b8d-b49c-45edaf928150
REPORT RequestId: 8393e7be-6a67-4b8d-b49c-45edaf928150  Init Duration: 0.31 ms  Duration: 119.65 ms       Billed Duration: 200 ms Memory Size: 128 MB     Max Memory Used: 128 MB
{"statusCode": 200, "body": "{\"message\": \"hello world\"}"}%     
```

### Step 7 - Deploy your application
```bash
sam deploy --guided
```

# Reference
> https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started-hello-world.html