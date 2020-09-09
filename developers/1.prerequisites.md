# Prerequisites

### Install Python3

Install Python 3 and pip3 on your specific machine. You can choose to run the integration on an AWS EC2 instance or on a local Linux machine. Skip to [Verify install](prerequisites.md#verify-install) if you already have these installed.

{% tabs %}
{% tab title="AWS EC2" %}
1. From the AWS EC2 console launch a new instance. We _recommend_ you use **Ubuntu Server 18.04 LTS \(HVM\), SSD Volume Type**
2. Connect to the EC2 Instance using SSH

   ```bash
   ssh -i "<user-private-key-file.pem>" ubuntu@<instance-public-DNS>
   ```

3. Update the package list

   ```bash
   sudo apt update
   ```

4. Install Python 3 and pip3 packages

   ```bash
   sudo apt install python3
   sudo apt install python3-pip
   ```
{% endtab %}

{% tab title="Ubuntu/Debian Linux" %}
{% hint style="warning" %}
To install _python3-pip_ you may need to install the "universe" repository running command

```bash
sudo add-apt-repository universe
```
{% endhint %}

1. install Python3

   ```bash
   sudo apt-get install python3
   ```

2. install pip3

   ```bash
   sudo apt-get install python3-pip
   ```
{% endtab %}
{% endtabs %}

### Verify install

Make sure that Python3 and Pip3 were installed correctly

```bash
python3 --version
pip3 --version
```



