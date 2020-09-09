# Setup C Virtual Device

Minimal code implementing an UniquID node. It can be used to add UniquID functionalities to the [AWS IoT C sdk](https://github.com/aws/aws-iot-device-sdk-embedded-C).  
Please refer to the [README](https://github.com/uniquid/uidagent-c/blob/master/README.md) in the repo for further informations.

### Install Prerequisites

Install these packages on your machine

```bash
sudo apt install cmake
sudo apt install libcurl4-openssl-dev
```

### How to integrate in the AWS IoT C sdk

1. Clone the AWS-IoT-sdk

   ```bash
   git clone https://github.com/aws/aws-iot-device-sdk-embedded-C.git -b v3.0.1
   ```

2. Clone this repository inside the external\_libs directory

   ```bash
   cd aws-iot-device-sdk-embedded-C/external_libs/
   git clone https://github.com/uniquid/uidagent-c.git
   ```

3. Clone the uidcore-c library

   ```bash
   git clone --recurse-submodules https://github.com/uniquid/uidcore-c.git
   ```

4. Clone the mbedtls code

   ```bash
   rm mbedTLS/README.txt
   git clone https://github.com/ARMmbed/mbedtls.git mbedTLS -b mbedtls-2.16.1
   ```

5. Apply the provided patch, IoT-sdk.patch

   ```bash
   cd ..
   git apply external_libs/uidagent-c/IoT-sdk.patch
   cd samples/linux/subscribe_publish_sample
   make
   ```

### Run the C device

1. Copy in the "certs" directory the CA certificates chain to authenticate the aws-mqtt-proxy and the uniquid mqtt broker. The file must be named rootCA.crt

   ```bash
   cp ../../../../caChain-xxxxxxxxxxxxxx.crt ../../../certs/rootCA.crt
   ```

   Press tab to find the right caChain file.

2. Copy the configuration file \([example file](https://github.com/uniquid/uidagent-c/blob/master/aws_device_cfg.json)\) \*\*

   ```bash
   cp ../../../../aws_device_cfg.json aws_device_cfg.json
   ```

3. Run the sample

   ```bash
   ./subscribe_publish_sample
   ```

\*\* The configuration can also be loaded from AWS\_AGENT\_CONFIG environment variable

```bash
export AWS_AGENT_CONFIG=$(cat path-to/aws_device_cfg.json)
```

The device will try to connect to the proxy every 10 seconds, for this reason an error message will be reported until you [create an UniquID Contract](https://uniquid.gitbook.io/uniquid/create-uniquid-contract) between the C device and the AWS-IOT-UNIQUID-AUTHORIZER to bind these two components.

{% hint style="warning" %}
Only when both the components will receive the contract from the blockchain, the device will be able to connect.
{% endhint %}

