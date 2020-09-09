# Remote Control App

### Requirements

* Node.js 8 \(not higher\) and npm
* make
* cmake
* g++

{% hint style="warning" %}
You should create a new terminal session before following steps.  The previous session you used has been customised for the DIGI toolchain and cannot be used to build a Node.js application.
{% endhint %}

### Installation

1. install requirements

   ```bash
   curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash - && sudo apt-get install -y nodejs
   sudo apt-get install -y make cmake g++
   ```

2. from the root of the project `dev-demos` 

   ```bash
   cd app/server
   ```

3. install package

   ```bash
   npm install
   ```

4. download and install latest Litecoin TestNet headers database \([more information](https://github.com/uniquid/uidcore-js#ltc-backup-cli-tool)\)

   ```bash
   npx ltc-backup install testnet -t nodeHome
   ```

### Run the application

1. set and export an environment variable with the information needed from the application. This information was prepared by the UniquID CLI when you run the `uniquid deploy aws` command. The information was stored on the file `~/aws_device_cfg.json` Run the command:

   ```bash
   export AWS_AGENT_CONFIG=$(cat ~/aws_device_cfg.json)
   ```

2. check the correct setup with:

   ```bash
   echo $AWS_AGENT_CONFIG
   ```

3. start the application

   ```bash
   npm start
   ```

4. [deploy a contract](https://uniquid.gitbook.io/uniquid/create-uniquid-contract) between the **board \(provider\)** and the **application \(user\)** with the following functions enabled:

   * 34 set led status \(on/off\)
   * 35 read led status \(on/off\)



   {% code title="contract.json" %}
   ```bash
   [
         {
             "provider": "tpubXXXXX",
             "user": "tpubYYYYY",
             "functions": [34, 35]
         }
   ]
   ```
   {% endcode %}

5. interact with the device
   1. if you are working on the EC2 instance 
      1. create a security group to open port `3000`
      2. open browser to `http://<ec2-ip-address>:3000`
   2. if you are working on your local machine
      1. open your browser on `http://localhost:3000`

