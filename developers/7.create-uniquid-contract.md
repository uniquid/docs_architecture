# Create UniquID Contract

### Login to organization

Navigate to directory where you saved the `credentials.json` file

```bash
cd /path/to/file/
```

You may need to re-login if the token has expired. 

```bash
uniquid login --credentials-file credentials.json
```

### Check devices

Check for the devices appear on the UniquID organization. This can be done with the following command:

```bash
uniquid list-devices
```

### Create contract file

Create a new file called `contract.json` with the following content. Be sure to replace the placeholders with your own information

{% code title="contract.json" %}
```javascript
[
  {
    "provider" : "tpub of the authorizer from the uniquid list-devices command",
    "user" : "tpub of the deivce from the uniquid list-devices command",
    "functions" : [32]
  }
]
```
{% endcode %}

{% hint style="warning" %}
The "provider" is the AWS-IOT-UNIQUID-AUTHORIZER and the "user" is the JS or C device.
{% endhint %}

### Create contract

Create and deploy the contract

```bash
uniquid create-contracts --input-json-file contract.json
```

Check contract list

```bash
uniquid list-contracts --output json
```

Look at the log from the device and check [here](https://us-east-2.console.aws.amazon.com/iot/home?region=us-east-2#/test) for published timestamps from the Device \(please note that the link provided is for the us-east-2 AWS region, you may need to switch to the one you have defined on _aws configure_\).

