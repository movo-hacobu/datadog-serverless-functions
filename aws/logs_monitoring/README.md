# Datadog Forwarder

AWS Lambda function to ship logs from S3 and CloudWatch, custom metrics and traces from Lambda functions to Datadog.

## Features

- Forward CloudWatch, ELB, S3, CloudTrail, VPC and CloudFront logs to Datadog
- Forward S3 events to Datadog
- Forward Kinesis data stream events to Datadog, only CloudWatch logs are supported
- Forward custom metrics from AWS Lambda functions via CloudWatch logs
- Forward traces from AWS Lambda functions via CloudWatch logs
- Generate and submit enhanced Lambda metrics (`aws.lambda.enhanced.*`) parsed from the AWS REPORT log: duration, billed_duration, max_memory_used, and estimated_cost

## Install

1. Deploy the [datadog-serverless](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=datadog-serverless&templateURL=https://dd-log-sam.s3.amazonaws.com/templates/3.0.0.yaml) CloudFormation stack in your AWS account and region.
1. You can find the installed Forwarder under the stack's "Resources" tab.
1. Set up triggers to the installed Forwarder either [manually](https://docs.datadoghq.com/integrations/amazon_web_services/?tab=allpermissions#manually-setup-triggers) or let Datadog [manage the triggers for you automatically](https://docs.datadoghq.com/integrations/amazon_web_services/?tab=allpermissions#automatically-setup-triggers).

## Update

### Upgrade to a different version

1. Find the [datadog-serverless (if you didn't rename it)](https://sa-east-1.console.aws.amazon.com/cloudformation/home#/stacks?filteringText=datadog) CloudFormation stack.
1. Update the stack using template `https://dd-log-sam.s3.amazonaws.com/templates/<VERSION>.yaml`. The latest version can be found [here](template.yaml#L7).

### Adjust forwarder settings

1. Find the [datadog-serverless (if you didn't rename it)](https://sa-east-1.console.aws.amazon.com/cloudformation/home#/stacks?filteringText=datadog) CloudFormation stack.
1. Update the stack using the current template.
1. Adjust parameter values.

Note: It's recommended to adjust forwarder settings through CloudFormation rather than directly editing the Lambda function.

## Notes

* For S3 logs, there may be some latency between the time a first S3 log file is posted and the Lambda function wakes up.
* Currently, the forwarder has to be deployed manually to GovCloud and China, and supports only log forwarding.
  1. Create a Lambda function using `aws-dd-forwarder-<VERSION>.zip` from the latest [releases](https://github.com/DataDog/datadog-serverless-functions/releases).
  1. Save your Datadog API key in AWS Secrets Manager, and set environment variable `DD_API_KEY_SECRET_ARN` with the secret ARN on the Lambda function.
  1. Configure [triggers](https://docs.datadoghq.com/integrations/amazon_web_services/?tab=allpermissions#send-aws-service-logs-to-datadog).