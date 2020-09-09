# Install AWS CLI

### Install AWS tool

```bash
sudo pip3 install awscli --upgrade
```

### Generate AWS IAM Access Key

Click [here](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-quick-configuration) for instructions on how to generate an access key from the AWS IAM console

### Configure CLI

Run the AWS CLI command

```bash
aws configure
```

Input the following parameters:

**AWS Access Key ID**: Downloaded from AWS IAM console.

**AWS Secret Access Key**: Downloaded from AWS IAM console.

**Default region name**: The user's preferred AWS Region

**Default output format**: \[Return/Enter\] to use the default.