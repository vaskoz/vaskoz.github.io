---
title: Arbitrary Binaries on AWS Lambda
---

Since the beginning of [AWS Lambda](https://aws.amazon.com/lambda/), developers could [launch a child process](https://github.com/eawsy/aws-lambda-go-shim) to execute any arbitrary binary on Linux. The release of the "go1.x" runtime for AWS Lambda simplifies the deployment of arbitrary binaries. Binaries need only be statically-linked and given execute permissions for all (chmod 444) to be successfully run on AWS Lambda.

Below I've embedded a piece of code to observe the Lambda environment.

I performed the following steps to create a Zip file and then uploaded that Zip file to Lambda.

```
GOOS=linux go build -o main lambda.go
zip deployment.zip main
```

{% gist df9b1c821951811098c876d44d681dd5 lambda.go %}

Here are the resulting CloudWatch logs for lambda.go.

{% gist df9b1c821951811098c876d44d681dd5 cloudwatch_logs_lambda_go.txt %}

And here is an example of a C++ program running in AWS Lambda.

I performed the following steps to create a Zip file and then uploaded that Zip file to Lambda.

```
g++ -static lambda.cpp -o main
chmod a+x main
zip deployment.zip main
```

{% gist df9b1c821951811098c876d44d681dd5 lambda.cpp %}

Here are the resulting CloudWatch logs for lambda.cpp.

{% gist df9b1c821951811098c876d44d681dd5 cloudwatch_logs_lambda_cpp.txt %}