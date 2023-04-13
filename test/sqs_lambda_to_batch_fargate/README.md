![Stability: Stable](https://img.shields.io/badge/stability-Stable-success.svg?style=for-the-badge)

# Batch Fargate Consumer Stack

The code in this directory deploys a CDK stack capable of running integration tests.

This stack can be deployed in isolation; however, it serves a purpose in this repository's [test automation architecture](../README.md).

Specifically, an SQS topic consumes messages from a cross-account SNS topic. SQS then triggers a Lambda function which submits a new AWS Batch job containing test commands.

![weathertop-comp-2.png](..%2Farchitecture_diagrams%2Fpng%2Fweathertop-comp-2.png)

git pu---
## System requirements
* npm (node.js)
* python 3.7  
* AWS access key & secret for AWS user with permissions to create resources listed above

### Environment variables
Before going any further, save your language name as an environment variable called `LANGUAGE_NAME`.

If your language is Java, you would use:
```
export LANGUAGE_NAME=javav2
```

Also, save the AWS Account ID of the AWS account that is currently emitting 
events that this stack will process.
```
export PRODUCER_ACCOUNT_ID=12345678901
```

Lastly, save the name of the SNS topic that will be producing the events mentioned above.
If created using [this Producer CDK code](../eventbridge_rule_with_sns_fanout/README.md) it will look something like this:
```
export FANOUT_TOPIC_NAME=ProducerStack-fanouttopic6EFF7954-pYvxBdNPbEWM
```
---

## CDK setup & deployment

First, you will need to install the AWS CDK:

```
sudo npm install -g aws-cdk
```

You can check the toolkit version with this command:

```
cdk --version
```

Now you are ready to create a virtualenv:

```
python3 -m venv .venv
```

Activate your virtualenv:

```
source .venv/bin/activate
```

Install the required dependencies:

```
pip install -r requirements.txt
```

At this point you can now synthesize the CloudFormation template for this code.

```
cdk synth
```

If everything looks good, go ahead and deploy!  This step will actually make
changes to your AWS cloud environment.  

```
cdk bootstrap
cdk deploy
```

To clean up, issue this command:

```
cdk destroy
```

To exit the virtualenv python environment:

```
deactivate
```

# Useful commands

 * `cdk ls`          list all stacks in the app
 * `cdk synth`       emits the synthesized CloudFormation template
 * `cdk deploy`      deploy this stack to your default AWS account/region
 * `cdk diff`        compare deployed stack with current state
 * `cdk docs`        open CDK documentation

---
This code has been tested and verified to run with AWS CDK 2.70.0 (build c13a0f1).
