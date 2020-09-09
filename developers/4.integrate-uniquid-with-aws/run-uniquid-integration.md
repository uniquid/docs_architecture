# Run UniquID integration

### Install Node.js

Node.js is needed to run UniquID deployment to AWS

```bash
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash - && sudo apt-get install -y nodejs
```

Then, test to see if version `8.x` is installed

```bash
node --version
```

### Deploy to AWS

Run the command to start integration of the UniquID system with the Amazon AWS IoT Hub:

```bash
uniquid deploy aws
```

You should respond yes to all the questions. Please accept the default options if you have no reason to change them.

Wait for the AWS-IOT-UNIQUID-AUTHORIZER device to appear in the list of devices. You can check it with this command:

```bash
uniquid list-devices
```

{% hint style="info" %}
To see a formatted output you can run

```bash
uniquid list-devices --output json
```
{% endhint %}

You can view events and actions from the UniquID IAM system in realtime by using the log command

```bash
uniquid log
```

You are now ready to [create a virtual device](https://uniquid.gitbook.io/uniquid/virtual-device) which demonstrates how an IoT device can be integrated with the AWS IoT Core and the UniquID IAM system.  If you prefer, you can now jump ahead and integrate a real [IoT hardware device](https://uniquid.gitbook.io/uniquid/hardware) with the Uniquid IAM system.