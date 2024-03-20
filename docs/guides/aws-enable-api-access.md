# Enabling AWS payer API access using bluectl

!!! note
    This guide is only applicable to Ripple users.

If you have registered your AWS payer account using CloudFormation as described [here](https://alphauslabs.github.io/docs/guides/aws-register-payer/), Alphaus will have a read-only API access to the payer's cost details such as RIs, SPs, etc. However, if you have some payer accounts that were registered to Ripple manually (before the CloudFormation support was released), Alphaus only has read access to the S3 bucket containing the CUR files. Although API access is optional, we recommend you to setup API access to allow us to have more accuracy in the calculation results. Without API access, our calculation engine, especially for trueunblended, uses CUR data for your RIs and SPs. This is only a best-effort basis as RI and SP information in the CUR are sometimes incomplete. API access will allow us to have a more accurate information about your payer's RIs, SPs, and other cost-related resources.

Make sure to install [`bluectl`](https://alphauslabs.github.io/docs/blueapi/bluectl/) first. Also make sure that you have the needed permissions to deploy [CloudFormation](https://aws.amazon.com/cloudformation/) templates on the payer account. The template used in this guide will create an IAM Role with read-only API access to the payer's cost details. You can check out the actual template [here](https://alphaus-cloudformation-templates.s3.ap-northeast-1.amazonaws.com/alphausdefaultcostaccess-v1.yml).

Open a terminal and run the following command (`012345678901` is a sample payer account ID):

``` sh
$ bluectl xacct create 012345678901 apionly
Open the link below in your browser and deploy:
https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/..
Confirm successful deployment? [Y/n]: 
```

Click on the URL link, or copy it to your browser. It will open the CloudFormation console with the parameters filled up. Leave the defaults, check the "acknowledge" checkbox, and click "Create stack".

Once the deployment is done and successful, return to the terminal above and press ++enter++ (`Y` is the default option). The deployment validation will begin. If there are no issues, validation details are displayed and the process is completed.

You can run the command below to check API access information about your payer accounts.

``` sh
$ bluectl xacct list
```
