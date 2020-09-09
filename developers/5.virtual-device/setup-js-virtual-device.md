# Setup JS Virtual Device

 Please refer to the [README ](https://github.com/uniquid/aws-virtual-device-js/blob/master/README.md)in the repo for further informations

### Install Prerequisites

Install these packages on your machine

```bash
sudo apt-get install -y cmake make g++
```

### AWS JS Device

1. Clone the aws-virtual-device-js repo:

   ```bash
   git clone https://github.com/uniquid/aws-virtual-device-js
   ```

2. cd to the project directory

   ```bash
   cd aws-virtual-device-js/
   ```

3. Install the package

   ```bash
   npm install
   ```

4.  Download and install latest Litecoin testnet headers Database \([more information](https://github.com/uniquid/uidcore-js#ltc-backup-cli-tool)\)

   ```bash
   npx ltc-backup install testnet -t data
   ```

### Run the JS Device

{% hint style="warning" %}
Please assure this step has been done before starting the device
{% endhint %}

1. Set and export an environment variable with the information needed from the virtual device. This information was prepared by the UniquID CLI when you run the `uniquid deploy aws` command. The information was stored on the file `~/aws_device_cfg.json` Run the command:

   ```bash
   export AWS_AGENT_CONFIG=$(cat ~/aws_device_cfg.json)
   ```

2. Check the correct setup with:

   ```bash
   echo $AWS_AGENT_CONFIG
   ```

3. In the `aws-virtual-device-js` directory, run the command

   ```bash
   npm start
   ```

The virtual device will start to check for a contract with the UniquID Authorizer until you [create an UniquID Contract](https://uniquid.gitbook.io/uniquid/create-uniquid-contract). When found, the device will start to try the connection to the AWS IoT. When the connection successful, device will start to publish on AWS IoT MQTT.

