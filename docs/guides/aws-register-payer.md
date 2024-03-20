# Registering AWS payer accounts using bluectl

!!! note
    This guide is only applicable to Ripple users.

Make sure to install [`bluectl`](https://alphauslabs.github.io/docs/blueapi/bluectl/) first. Also make sure that you have the needed permissions to deploy [CloudFormation](https://aws.amazon.com/cloudformation/) templates on the payer account. The template used in this guide will setup a new [CUR](https://docs.aws.amazon.com/cur/latest/userguide/what-is-cur.html) export definition to an S3 bucket. It will also create an IAM Role with read access to the target bucket alongside read-only API access to the payer's cost details such as RIs, SPs, etc. You can check out the actual template [here](https://alphaus-cloudformation-templates.s3.ap-northeast-1.amazonaws.com/alphauscurexportdef-v1.yml).

Open a terminal and run the following command (`012345678901` is a sample payer account ID):

``` sh
$ bluectl xacct create 012345678901
Open the link below in your browser and deploy:
https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/..
Confirm successful deployment? [Y/n]: 
```

Click on the URL link, or copy it to your browser. It will open the CloudFormation console with the parameters filled up. Check the "acknowledge" checkbox, and click "Create stack".

Once the deployment is done and successful, return to the terminal above and press ++enter++ (`Y` is the default option). The deployment validation will begin. If there are no issues, registration details are displayed and the process is completed.

## Different region for S3 bucket

By default, the CloudFormation stack will be deployed to `us-east-1` region. This is because, at the time of this writing, AWS only supports CUR export definition on that region. If you prefer to have your S3 bucket located on a different region however, you can do the following steps.

First, create an S3 bucket using the command below. When in the CloudFormation console, make sure that you are in your desired region when deploying the stack. You can check out the actual template [here](https://alphaus-cloudformation-templates.s3.ap-northeast-1.amazonaws.com/alphauscurexportbucket-v1.yml).

``` sh
$ bluectl xacct create 012345678901 s3only
```

Once the bucket is created, deploy the main CloudFormation template using the command below. In the CloudFormation console, select the "USE_EXISTING" parameter, and update the S3 bucket name (`CurS3BucketName`) and region (`CurS3BucketRegion`) with the values used in the first deployment.

``` sh
$ bluectl xacct create 012345678901
```
